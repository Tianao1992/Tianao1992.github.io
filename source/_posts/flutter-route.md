---
title: flutter 路由相关
date: 2022-03-11 15:47:01
index_img: /img/ios/apple.jpg
categories:
- flutter
tags:
- flutter
---
### 认识Flutter路由

路由的概念由来已久，包括网络路由、后端路由，到现在广为流行的前端路由
 - 无论路由的概念如何应用，它的核心是一个路由映射表
 - 比如：名字 detail 映射到 DetailPage 页面等
 - 有了这个映射表之后，我们就可以方便的根据名字来完成路由的转发（在前端表现出来的就是页面跳转
在Flutter中，路由管理主要有两个类：Route和Navigator

### Route

Route：一个页面要想被路由统一管理，必须包装为一个Route
但是Route是一个抽象类，所以它是不能实例化的
- 在上面有一段注释，让我们查看MaterialPageRoute来使用

```
 /// See [MaterialPageRoute] for a route that replaces the
/// entire screen with a platform-adaptive transition.
abstractclass Route<T> {
}
```
事实上MaterialPageRoute并不是Route的直接子类

```
MaterialPageRoute -> PageRoute -> ModalRoute -> TransitionRoute -> OverlayRoute -> Route

```

### Navigator

Navigator：管理所有的Route的Widget，通过一个stack来进行管理的
我们开发中使用的MaterialApp、CupertinoApp、WidgetsApp它们默认是有插入Navigator的
所以，我们在需要的时候，只需要直接使用即可
```
Navigator.of(context)
```
常见方法
```
// 路由跳转：传入一个路由对象
Future<T> push<T extendsObject>(Route<T> route)

// 路由跳转：传入一个名称（命名路由）
Future<T> pushNamed<T extendsObject>(
  String routeName, {
    Object arguments,
  })

// 路由返回：可以传入一个参数
bool pop<T extendsObject>([ T result ])

```

### 命名路由使用

我们可以通过创建一个新的Route，使用Navigator来导航到一个新的页面，但是如果在应用中很多地方都需要导航到同一个页面（比如在开发中，首页、推荐、分类页都可能会跳到详情页），那么就会存在很多重复的代码
在这种情况下，我们可以使用命名路由（named route）
- 命名路由是将名字和路由的映射关系，在一个地方进行统一的管理
- 有了命名路由，我们可以通过Navigator.pushNamed() 方法来跳转到新的页面

命名路由在哪里管理呢？可以放在MaterialApp的 **initialRoute** 和 **routes** 中
- initialRoute：设置应用程序从哪一个路由开始启动，设置了该属性，就不需要再设置home属性了
- routes：定义名称和路由之间的映射关系，类型为Map<String, WidgetBuilder>

修改MaterialApp中的代码
```
return MaterialApp(
  title: 'Flutter Demo',
  theme: ThemeData(
    primarySwatch: Colors.blue, splashColor: Colors.transparent
  ),
  initialRoute: "/",
  routes: {
    "/home": (ctx) => HYHomePage(),
    "/detail": (ctx) => HYDetailPage()
  },
);
```

修改跳转中代码
```
_onPushTap(BuildContext context) {
  Navigator.of(context).pushNamed("/detail");
}
```

在开发中，为了让每个页面对应的routeName统一，我们通常会在每个页面中定义一个路由的常量来使用

```
class HomePage extends StatefulWidget {
  staticconstString routeName = "/home";
}

class DetailPage extends StatelessWidget {
  staticconstString routeName = "/detail";
}
```

修改MaterialApp中routes的key
```
initialRoute: HYHomePage.routeName,
routes: {
  HomePage.routeName: (ctx) => HomePage(),
  DetailPage.routeName: (ctx) => DetailPage()
},
```

### 参数传递
因为通常命名路由，我们会在定义路由时，直接创建好对象，比如DetailPage()
那么，命名路由如果有参数需要传递呢？
pushNamed时，如何传递参数
```
_onPushTap(BuildContext context) {
  Navigator.of(context).pushNamed(DetailPage.routeName, arguments: "a home message of naned route");
}
```
在DetailPage中，如何获取到参数呢？

在build方法中ModalRoute.of(context)可以获取到传递的参数
```
  Widget build(BuildContext context) {
    // 1.获取数据
    final message = ModalRoute.of(context).settings.arguments;
  }
```

#### 路由钩子

- onGenerateRoute
假如我们有一个AboutPage，也希望在跳转时，传入对应的参数message，并且已经有一个对应的构造方法

但是我们继续使用routes中的映射关系，就不好进行配置了，因为AboutPage必须要求传入一个参数；

这个时候我们可以使用onGenerateRoute的钩子函数：

当我们通过pushNamed进行跳转，但是对应的name没有在routes中有映射关系，那么就会执行onGenerateRoute钩子函数；
- 我们可以在该函数中，手动创建对应的Route进行返回；
- 该函数有一个参数RouteSettings，该类有两个常用的属性：
- name: 跳转的路径名称
- arguments：跳转时携带的参

```
onGenerateRoute: (settings) {
  if (settings.name == "/about") {
    return MaterialPageRoute(
      builder: (ctx) {
        return AboutPage(settings.arguments);
      }
    );
  }
  returnnull;
},
```

- onUnknownRoute
如果我们打开的一个路由名称是根本不存在，这个时候我们希望跳转到一个统一的错误页面。

比如下面的abc是不存在有对应的页面的

- 如果没有进行特殊的处理，那么Flutter会报错

```
RaisedButton(
  child: Text("打开未知页面"),
  onPressed: () {
    Navigator.of(context).pushNamed("/abc");
  },
)
```

我们可以创建一个错误的页面：
```
class UnknownPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("错误页面"),
      ),
      body: Container(
        child: Center(
          child: Text("页面跳转错误"),
        ),
      ),
    );
  }
}
```

并且设置onUnknownRoute
```
onUnknownRoute: (settings) {
  return MaterialPageRoute(
    builder: (ctx) {
      return UnknownPage();
    }
  );
},
```