<h1 align="center">nuxt.js基础搭建过程</h1>

nuxt.js是解决服务端渲染的组件，在Vue的[官网](https://cn.vuejs.org/v2/guide/ssr.html)都推荐nuxtjs:

>从头搭建一个服务端渲染的应用是相当复杂的。幸运的是，我们有一个优秀的社区项目 [Nuxt.js](https://nuxtjs.org/) 让这一切变得非常简单。Nuxt 是一个基于 Vue 生态的更高层的框架，为开发服务端渲染的 Vue 应用提供了极其便利的开发体验。更酷的是，你甚至可以用它来做为静态站生成器。推荐尝试。



## 一、准备工作

```html
安装node.js、npm；
```

安装vue-cli：

`-g`是全局安装的意思，安装一次即可。

```xml
npm install vue-cli -g
```

## 二、安装nuxt.js项目

在一个空文件夹中运行一下命令：

```
vue init nuxt/starter
```

这个动作一般在VSCode；里面操作方便，安装的时候会先下载模板（可能要一会），下好模板之后会询问一下项目名称、描述等，一路`Enter`即可；

如果是前端解决跨域问题，则需要安装：

```xml
npm i @gauseen/nuxt-proxy -D
```

这里前后端都是我自己写，所以可以不用安装，而是在后端controller方法添加`@CrossOrigin`注解即可。

必装——官方推荐的ajax封装组件`axios`：

```powershell
npm install axios --save
```

可以用npm axios -v查看版本。



##  三、项目目录结构

安装好之后，看看目录结构：

### .nuxt

这是nuxtjs生成文件的文件夹，一般不用改；

### assets

静态资源文件夹

### components

组件

### layouts

布局

### node_modules

依赖包存放处

### pages

放页面的地方

### plugins

自定义下载的`JavaScript插件`都放在这里

### static

静态资源文件

### store

状态管理？

### eslintrc.js(文件)

代码规范规则，大公司才用

### nuxt.config.js（重要文件）

这里面的配置可以理解为通用配置。

### package.json

也是配置文件，比如改端口号：

```json
     "config": {
        "descriptions": "这个参数是我自己写的，因为json文件不能写注释，下面的是配置端口号",
        "nuxt": {
            "host": "192.168.1.103",
            "port": "1818"
        }
    }
```

## 四、多页统一配置

传统单页应用，只需要引入js、css这些就能直接用了，如果我们想统一引用呢？如何只配置一次就可以实现全部页面都引用（比如css）？

测试方法步骤：

### 1）准备一个css

在文件夹`assets`下面创建一个`testcss.css`的文件；写入`html{color: red !important;}`；

加`!important`的作用是因为有些页面已经有默认样式的情况下，有时候会替换不了，加了!important之后就可以强制替换（？！）；

### 2）在nuxt.config.js中配置

在nuxt.config.js中的head{}中紧跟着加一个css{}配置：

```html
head:{省略..........},css: ['~assets/testcss.css'],
```

其中波浪线`~`的作用是省略写项目路径，直接找到项目路径下的`assets`文件夹。

> 这样就能将`testcss.css`这个样式文件统一配置到所有的页面了。



## 五、路由实现页面跳转

这里要理解前端的`路由`的概念：

在nuxt创建的这个项目结构里面，pages下面的文件夹名字就是跳转的路径，比如pages文件夹下面有一个about的文件夹：`pages/about/index.vue`，那么当我们访问一个`/about`的时候，就能直接访问到这个`index.vue`文件，这里nuxt.js推荐我们用`<nuxt-link>`标签替换传统的`a`标签。

```vue
<template>
  <div>
    <ul>
      <li>
        <nuxt-link :to="{name:'about'}">跳转到about.index页</nuxt-link>
      </li>
      <li>
        <nuxt-link :to="{name:'news'}">news.index页</nuxt-link>
      </li>
    </ul>
  </div>
</template>
```

路由文件在`.nuxt\router.js`这个位置可以点进去看看。

## 六、参数传递

```vue
<nuxt-link :to="{name:'index'}">返回HOME</nuxt-link>
```

页面跳转直接的参数是如何传递的？这里是`<nuxt-link>`标签的语法:

只需要在`:to={}`的参数中添加`params`属性即可，比如：

```vue
<nuxt-link :to="{name:'about',params:{testId:1234,testName:'示例文字'}}">跳转到about.index页</nuxt-link>
```

那么我们在跳转之后的接收页面获取方式和单页应用是一样的：

直接接收或者`<script></script>`里面接收都行：

```vue
<p>接收来自首页的参数: {{$route.params.testId}}</p>
<p>接收来自首页的参数: {{$route.params.testName}}</p>
```

可以看出`$route`对象和它的属性`params`的作用。



## 七、动态路由和参数校验

（需要去）官网文档地址：[动态路由](https://www.nuxtjs.cn/guide/routing#%E5%8A%A8%E6%80%81%E8%B7%AF%E7%94%B1)详细看这里不多展开，根据文档的提示，其设计思路是：

> 在 Nuxt.js 里面定义带参数的动态路由，需要创建对应的**以下划线作为前缀**的 Vue 文件 或 目录。

比如在pages/news文件夹下面创建`_id.vue`文件，和`index.vue`同级：

```vue
news
	_id.vue
	index.vue
```

现在我们希望在`news/index.vue`页面跳转到`_id.vue`页面,这里也有2中写法，其中`a`标签的写法很简单就是restFul风格：

在`news/index.vue`中：

```vue
<a href="/news/123">跳转到动态路由页（/news/123）</a>
<a href="/news/啊啊啊啊">跳转到动态路由页（/news/啊啊啊啊）</a>
```

特别注意`<nuxt-link>`标签的写法：

```vue
<nuxt-link :to="{name:'news-id',params:{id:123}}">跳转到动态路由页（/news/123）</nuxt-link>
<nuxt-link :to="{name:'news-id',params:{id:'啊啊啊啊啊'}}">跳转到动态路由页（/news/啊啊啊啊）</nuxt-link>
```

可以看出是`文件夹名称+横杠`作为跳转的页面，同时要在`params`写id参数的值。



`_id.vue`的内容是：

```vue
<template>
  <div>
    <h1>正位于news文件夹下的_id.vue页面</h1>
    <nuxt-link :to="{name:'index'}">返回首页</nuxt-link>
    <br />
    <p>接收news/index.vue页面传递的参数： {{$route.params.id}}</p>
  </div>
</template>
<script>
export default {
  validate({ params }) {
    // 必须是number类型
    return /^\d+$/.test(params.id); //这里/^\d+$/.test(params.id)的返回值是true/false
    // 如果return true，就会正常进入页面，如果false则跳错误页面
  }
};
</script>
```

留意到`{{$route.params.id}}`这个读取参数的写法，它能识别到`<a href="/news/123">跳转到动态路由页（/news/123）</a>`中的参数`123`，个人理解因为这是restFul API的写法，那么它的基础就是页面和页面之间的跳转的，那么route是否可以理解成后端项目中的`HttpServletRequest`？





校验的内容在下面讲 ↓

## 八、路由参数校验

官网文档地址[路由参数校验](https://www.nuxtjs.cn/guide/routing#%E8%B7%AF%E7%94%B1%E5%8F%82%E6%95%B0%E6%A0%A1%E9%AA%8C)页面找到`#路由参数校验`板块	:

> Nuxt.js 可以让你在动态路由组件中定义参数校验方法。
>
> 举个例子： `pages/users/_id.vue`
>
> ```
> export default {
>   validate ({ params }) {
>     // 必须是number类型
>     return /^\d+$/.test(params.id)
>   }
> }
> ```
>
> 如果校验方法返回的值不为 `true`或`Promise`中resolve 解析为`false`或抛出Error ， Nuxt.js 将自动加载显示 404 错误页面或 500 错误页面。
>
> 想了解关于路由参数校验的信息，请参考 [页面校验API](https://www.nuxtjs.cn/api/pages-validate)。

这里测试如下：

在`pages/news/index.vue`文件中:

```vue
<a href="/news/123">跳转到动态路由页</a>
<a href="/news/啊啊啊啊">跳转到动态路由页</a>
```

在对应的`pages/news/_id.vue`文件中：

```vue
<template>
  <div>
    <h1>正位于news文件夹下的_id.vue页面</h1>
    <nuxt-link :to="{name:'index'}">返回首页</nuxt-link>
    <br />
    <p>接收news/index.vue页面传递的参数： {{$route.params.id}}</p>
  </div>
</template>
<script>
export default {
  validate({ params }) {
    // 必须是number类型
    return /^\d+$/.test(params.id); //这里/^\d+$/.test(params.id)的返回值是true/false
    // 如果return true，就会正常进入页面，如果false则跳错误页面
  }
};
</script>
```

个人理解/猜测，页面A>>>跳页面B的处理逻辑：

A>>>路由route>>>B的`export default`>>>B的`template`

> 点击效果和逾期的一致。



## 九、过渡动画效果

官网文档地址：[过渡动画效果](https://www.nuxtjs.cn/guide/routing#%E5%8A%A8%E6%80%81%E5%B5%8C%E5%A5%97%E8%B7%AF%E7%94%B1)

> Nuxt.js 使用 Vue.js 的`<transition>`组件来实现路由切换时的过渡动效。

> > 本模块暂时不打算用，后面想用的时候再回头看看；



## 十、默认模版和默认布局

### 默认模板

这个应该是对应官网文档中的[视图](https://www.nuxtjs.cn/guide/views)模块，官网文档写的很明白了：

>> 你可以定制化 Nuxt.js 默认的应用模板。
>
>定制化默认的 html 模板，只需要在src文件夹下（默认是应用根目录）创建一个 `app.html` 的文件。
>
>默认模板为：
>
>```vue
><!DOCTYPE html>
><html {{ HTML_ATTRS }}>
>  <head {{ HEAD_ATTRS }}>
>    {{ HEAD }}
>  </head>
>  <body {{ BODY_ATTRS }}>
>    {{ APP }}
>  </body>
></html>
>```
>
>举个例子，你可以修改模板添加 IE 的条件表达式：
>
>```vue
><!DOCTYPE html>
><!--[if IE 9]><html lang="en-US" class="lt-ie9 ie9" {{ HTML_ATTRS }}><![endif]-->
><!--[if (gt IE 9)|!(IE)]><!--><html {{ HTML_ATTRS }}><!--<![endif]-->
>  <head {{ HEAD_ATTRS }}>
>    {{ HEAD }}
>  </head>
>  <body {{ BODY_ATTRS }}>
>    {{ APP }}
>  </body>
></html>
>```

个人理解：也就是说你在这个模板里面加的东西，每个页面都会展示，比如我们在`{{ APP }}` 的前后分别加一行P标签，然后在之前写个各个页面都点开会发现都有这2行自定义添加的P标签。

所以可以看出之前写的`index.vue`或者`_id.vue`这些文件其实就是相当于`{{ APP }}`占的位置。



**这里有个小坑：改这个默认模板`app.html`需要重启服务器。**



### 默认布局

含义：默认布局指的是`<template></template>`之内的内容，之外的它处理不了.默认布局文件位于layouts文件夹中的`layouts\default.vue`。

```vue
<template>
  <div>
    <!-- 这里在<nuxt/>的前后加上p标签进行测试 -->
    <p>这是在默认布局里面加的标签-前</p>
    <nuxt />
    <p>这是在默认布局里面加的标签-后</p>
  </div>
</template>
```

这里不用重启服务器，可以查看各个画面都是有效果了。

## 十一、自定义错误页

直接看官网文档【[页面](https://www.nuxtjs.cn/guide/views#%E9%A1%B5%E9%9D%A2)】，有很详细的解析和例子：

>> 你可以通过编辑 `layouts/error.vue` 文件来定制化错误页面.
>
>**警告:** 虽然此文件放在 `layouts` 文件夹中, 但应该将它看作是一个 **页面(page)**.
>
>这个布局文件不需要包含 `<nuxt/>` 标签。你可以把这个布局文件当成是显示应用错误（404，500等）的组件。
>
>默认的错误页面源码在 [这里](https://github.com/nuxt/nuxt.js/blob/dev/packages/vue-app/template/components/nuxt-error.vue).
>
>举一个个性化错误页面的例子 `layouts/error.vue`:
>
>```vue
><template>
>  <div class="container">
>    <h1 v-if="error.statusCode === 404">页面不存在</h1>
>    <h1 v-else>应用发生错误异常</h1>
>    <nuxt-link to="/">首 页</nuxt-link>
>  </div>
></template>
>
><script>
>export default {
>  props: ['error'],
>  layout: 'blog' // 你可以为错误页面指定自定义的布局
>}
></script>
>```





## 十二、页面组件及其配置项

对应【[官网文档](https://www.nuxtjs.cn/guide/views#%E9%A1%B5%E9%9D%A2)】中的`页面`、`HTML头部`、`默认Meta标签`三个板块。

个人理解：nuxt.js为了每一个`XXX.vue`文件都默认配置了一些页面属性，让我们平时写页面时只关注具体内容写在`template`里面即可，但是如果我们要对某个页面进行个性化配置的时候，我们是如何修改这些属性的呢？

比如在`_id.vue`页，这个页面我打算用来做类似新闻的`详情页`，回顾一下它的基本结构：

```vue
<template>
	//省略详细内容..............
</template>

<script>
export default {
	//省略详细内容..............
};
</script>
```

根据官网【页面】模块的文档的提示，

> 页面组件实际上是 Vue 组件，只不过 Nuxt.js 为这些组件添加了一些特殊的配置项（对应 Nuxt.js 提供的功能特性）以便你能快速开发通用应用。



​       所以个人理解为：nuxt.js帮忙封装了常用的一些`全局变量`和`全局方法`,我们在`<script></script>`里面直接调用然后结果返回给到`<template>`进行渲染。

前面的笔记其实已经接触到了就是`validate()`方法，官网还列举了其他很多，后面可以慢慢都用上：



>| 属性名      | 描述                                                         |
>| ----------- | ------------------------------------------------------------ |
>| asyncData   | 最重要的一个键, 支持 [异步数据处理](https://www.nuxtjs.cn/guide/async-data)，另外该方法的第一个参数为当前页面组件的 [上下文对象](https://www.nuxtjs.cn/api#上下文对象)。 |
>| fetch       | 与 `asyncData` 方法类似，用于在渲染页面之前获取数据填充应用的状态树（store）。不同的是 `fetch` 方法不会设置组件的数据。详情请参考 [关于fetch方法的文档](https://www.nuxtjs.cn/api/pages-fetch)。 |
>| head        | 配置当前页面的 Meta 标签, 详情参考 [页面头部配置API](https://www.nuxtjs.cn/api/pages-head)。 |
>| layout      | 指定当前页面使用的布局（`layouts` 根目录下的布局文件）。详情请参考 [关于 布局 的文档](https://www.nuxtjs.cn/api/pages-layout)。 |
>| loading     | 如果设置为`false`，则阻止页面自动调用`this.$nuxt.$loading.finish()`和`this.$nuxt.$loading.start()`,您可以手动控制它,请看[例子](https://nuxtjs.org/examples/custom-page-loading),仅适用于在nuxt.config.js中设置`loading`的情况下。请参考[API配置 `loading` 文档](https://www.nuxtjs.cn/api/configuration-loading)。 |
>| transition  | 指定页面切换的过渡动效, 详情请参考 [页面过渡动效](https://www.nuxtjs.cn/api/pages-transition)。 |
>| scrollToTop | 布尔值，默认: `false`。 用于判定渲染页面前是否需要将当前页面滚动至顶部。这个配置用于 [嵌套路由](https://www.nuxtjs.cn/guide/routing#嵌套路由)的应用场景。 |
>| validate    | 校验方法用于校验 [动态路由](https://www.nuxtjs.cn/guide/routing#动态路由)的参数。 |
>| middleware  | 指定页面的中间件，中间件会在页面渲染之前被调用， 请参考 [路由中间件](https://www.nuxtjs.cn/guide/routing#中间件)。 |
>
>关于页面配置项的详细信息，请参考 [页面 API](https://www.nuxtjs.cn/api)。



下面就测试几个配置项，其中常用的就是`meta`(SEO用)、**`asyncData`**异步数据处理、head等。

### validate()

上面第八节已经详细讲了，这里不展开.

在想要自定义的`_id.vue`页面：

```vue
<template>
</template>
<script>
export default {
  validate({ params }) {
  	return true/false;
  }
};
</script>
```

### **head()**

要彻底理解head，需要回顾一下`nuxt.config.js`文件，它里面全局配置了一个`head:{}`:

```vue
    head: {
        title: 'blog-views',
        meta: [
            { charset: 'utf-8' },
            { name: 'viewport', content: 'width=device-width, initial-scale=1' },
            { hid: 'description', name: 'description', content: 'Nuxt.js project' }
        ],
        link: [
            { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }
        ]
    },
```

如果我们想用自定义的`meta`，就需要去新增或者覆盖默认的配置，新增的规则很简单，只要你按照它的JSON格式，在你想要新增的页面（.vue文件）里面配置即可，比如我就在`_id.vue`文件中：

```vue
<template>
</template>
<script>
export default {
  validate({ params }) {
	//省略........
  }
};
  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: "自定义meta",
          name: "news1",
          content: "这是一个news标签"
        }
      ]
    };
  }
</script>
```

其`meta`对象是一个json数组：

```json
meta：[
    {hid:"",name:"",content:""},
    {hid:"",name:"",content:""},
    {hid:"",name:"",content:""},
    .....
]
```

这样配置之后页面就能添加新的meta且不覆盖默认配置的，我们去页面F12看看：

```html
<head>
    <title>blog-views</title>
    <meta data-n-head="ssr" charset="utf-8">
    <meta data-n-head="ssr" name="viewport" content="width=device-width, initial-scale=1">
    <meta data-n-head="ssr" data-hid="description" name="description" content="Nuxt.js project">
    <meta data-n-head="ssr" data-hid="自定义meta" name="news1" content="这是一个news标签">
	......
</head>    
```

想要覆盖的话，只需要定义相同的`hid`即可，默认的hid=description，我们自定义的hid也要他等于description即可。



### asyncData()

asyncData()方法是异步加载数据，是必须掌握的方法（/配置项）。【[官网文档](https://www.nuxtjs.cn/guide/async-data)】可能更详细,这里测试几组实例。

> *Nuxt.js 扩展了 Vue.js，增加了一个叫* `asyncData` *的方法，使得我们可以在设置组件的数据之前能异步获取或处理数据。*

测试步骤：

*前面确保已安装了axios。*

1）创建数据仓库；（自己搭了个SpringBoot-Web）>>>>>>>`controller`都加上`@CrossOrigin`用于后端解决跨域问题；

2）新建一个测试页面: `pages\asyncData.vue`；

3）在`pages\asyncData.vue`页面引入axios；

4）2种方式调用axios获取数据；

```vue
//先看看拿到数据后展示的部分：
<template>
  <div>
    <h1>当前在异步请求数据页`pages\asyncData.vue`</h1>
    <br />
    <div>{{ name }}</div>
    <div>
      <h3>以下是从SpringBoot后台获取的数据</h3>
      <br />
      <p>姓名： {{info.name }}</p>
      <p>年龄： {{info.age }}</p>
      <p>爱好： {{info.love }}</p>
    </div>
  </div>
</template>
<script>
	//.........
</script>
```

#### 方式1：promise

```vue
<script>
import axios from "axios";
export default {
  components: {},
  data() {
    return {
      name: "Hello World!"
    };
  },
  //   2种方法之一：promise
  asyncData() {
    return axios.get("http://localhost:6688/data").then(res => {
      return { info: res.data };
    });
  }
};
</script>
```

理解：

axios.get()获取数据后，通过then方法，将结果集return，其中nuxtjs的设计逻辑是：`asyncData()`方法的返回值是给到`data()`方法中的return部分的，也就是说：

```vue
  data() {
    return {
      name: "Hello World!"
    };
  }
```

接收了asyncData的`info`参数后，也就是：

```vue
  data() {
    return {
      name: "Hello World!",
	  info: "对应的值"	//《《《《《《《《《《《《《
    };
  }
```

这里不用一开始就写上info:{}，这是隐式的。

且这是主键渲染之前的动作，是调用不了this的。

#### 方式2：async/await方法（推荐多练）

这种方式比较优雅

```vue
  async asyncData() {
    let { data } = await axios.get("http://localhost:6688/data");//定义一个data变量，它的值是由axios.get()方法获取，await的意思是等待结果执行完毕才返回值；
    return { info: data };//将结果return，也是return到data()里面；
  }
```



## 十三、静态资源文件和生产静态HTML

如何引入图片：

```html
传统方式
<img src="../static/innocent.jpg" alt="无辜" style="max-width: 100px; max-height: 100px;" />
```

但是nuxt十分贴心的提供了`~`的方式替代冗长的代码，可以直接写：

```html
<img src="~static/innocent.jpg" alt="无辜" style="max-width: 100px; max-height: 100px;" /></div>
```

如果是在css中引用，和传统是一样的：

```html
<style>
.imgLocation {
  width: 100px;
  height: 100px;
  background-image: url("~static/innocent.jpg");
}
</style>
```

#### 生成静态文件

```powershell
npm run generate
```

执行完后会在项目下生成一个`dist`的文件夹，里面存放的是静态资源文件，这个文件是需要放到服务器才能运行，视频提供了一种快速测试的方案，首先把dist文件夹复制到其他地方比如桌面，最好改名字比如testblog，为了方便使用powershell还是用VSCode打开它，然后控制台安装一个（轻量级？的）服务器：

```powershell
npm install -g live-server
```

*如果安装过无须重复安装。*

安装完之后，使用以下命令启动服务器：

```powershell
live-server
```

这个服务器的默认端口是8080，浏览器访问http://127.0.0.1:8080/即可，一般启动服务就会自动弹出。

测试之前的链接都是OK的包括异步加载。





## 视频教程链接

本学习文档是根据B站JSPang老师的视频【[nuxtjs](https://www.bilibili.com/video/BV194411G74t?from=search&seid=542615147458306907)】进行记录。

## 其他

运行过程中`CTRL+C`可以退出运行.

export 和 export default 的区别 - 枫鸟~ - 博客园 https://www.cnblogs.com/fanyanzhao/p/10298543.html

删除node_modules文件夹后重新安装依赖：https://jingyan.baidu.com/article/4f7d57127e514f5b21192766.html

理解 JavaScript 的 async/await - 边城客栈 - SegmentFault 思否 https://segmentfault.com/a/1190000007535316

npm中全局安装和本地安装的区别 http://www.mamicode.com/info-detail-1758031.html