# JavaScript 的这 4 个运算符，让代码更干净简洁！！！

[mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzUzNTk3MjE2Ng==&mid=2247501534&idx=1&sn=9f880cd10d9ccc19d3147e8c418dc571&chksm=faffd6f7cd885fe1df636ebb3c655f704b12fae07d7dbf0256bf38322a3817c047e244d81486&mpshare=1&scene=1&srcid=1010vLpKUH2FmVvVciqMwfCJ&sharer_sharetime=1665413737217&sharer_shareid=b8d5da03cbe546fb54510ac993e581cf#rd)前端开发爱好者

点击下方“前端开发爱好者”，选择“设为星标”

第一时间关注技术干货！

ECMAScript 发展进程中，会有很多功能的更新，比如销毁，箭头功能，模块，它们极大的改变 JavaScript 编写方式，可能有些人喜欢，有些人不喜欢，但像每个新功能一样，我们最终会习惯它们。

新版本的 ECMAScript 引入了三个新的逻辑赋值运算符：空运算符，AND 和 OR 运算符，这些运算符的出现，也是希望让我们的代码更干净简洁，下面分享几个优雅的 JavaScript 运算符使用技巧。

#### 一、可选链接运算符【？.】

**可选链接运算符（Optional Chaining Operator）**处于 ES2020 提案的第 4 阶段，因此应将其添加到规范中。它改变了访问对象内部属性的方式，尤其是深层嵌套的属性。它也可以作为 TypeScript 3.7 + 中的功能使用。

相信大部分开发前端的的小伙伴们都会遇到 null 和未定义的属性。JS 语言的动态特性使其无法不碰到它们。特别是在处理嵌套对象时，以下代码很常见：

    if(data&&data.children&&data.children[0]&&data.children[0].title){//Ihaveatitle!}

上面的代码用于 API 响应，我必须解析 JSON 以确保名称存在。但是，当对象具有可选属性或某些配置对象具有某些值的动态映射时，可能会遇到类似情况，需要检查很多边界条件。

这时候，如果我们使用可选链接运算符，一切就变得更加轻松了。它为我们检查嵌套属性，而不必显式搜索梯形图。我们所要做的就是使用 “？” 要检查空值的属性之后的运算符。我们可以随意在表达式中多次使用该运算符，并且如果未定义任何项，它将尽早返回。

**对于静态属性**用法是：

    object?.property

**对于动态属性**将其更改为：

    object?.[expression]

上面的代码可以简化为：

    lettitle=data?.children?.[0]?.title;

然后，如果我们有:

    letdata;console.log(data?.children?.[0]?.title)//undefineddata={children:[{title:'codercao'}]}console.log(data?.children?.[0]?.title)//codercao

这样写是不是更加简单了呢？由于操作符一旦为空值就会终止，因此也可以使用它来有条件地调用方法或应用条件逻辑

    constconditionalProperty=null;letindex=0;console.log(conditionalProperty?.[index++]);//undefinedconsole.log(index);//0

**对于方法**的调用你可以这样写

    object.runsOnlyIfMethodExists?.()

例如下面的`parent`对象，如果我们直接调用`parent.getTitle()`, 则会报`Uncaught TypeError: parent.getTitle is not a function`错误，`parent.getTitle?.()`则会终止不会执行

    letparent={name:"parent",friends:["p1","p2","p3"],getName:function(){console.log(this.name)}};parent.getName?.()//parentparent.getTitle?.()//不会执行

**与无效合并一起使用**

提供了一种方法来处理未定义或为空值和表达提供默认值。我们可以使用`??`运算符，为表达式提供默认值

    console.log(undefined??'codercao');//codercao

因此，如果属性不存在，则可以将无效的合并运算符与可选链接运算符结合使用以提供默认值。

    lettitle=data?.children?.[0]?.title??'codercao';console.log(title);//codercao

#### 二、逻辑空分配（?? =）

    expr1??=expr2

逻辑空值运算符仅在 nullish 值（`null`或者`undefined`）时才将值分配给 expr1，表达方式：

    x??=y

可能看起来等效于：

    x=x??y;

但事实并非如此！有细微的差别。

空的合并运算符（??）从左到右操作，如果 x 不为**nullish 值**则中表达式不执行。因此，如果 x 不为`null`或者`undefined`，则永远不会对表达式`y`进行求值。如果`y`是一个函数，它将根本不会被调用。因此，此逻辑赋值运算符等效于

    x??(x=y);

#### 三、逻辑或分配（|| =）

此逻辑赋值运算符仅在左侧表达式为**falsy 值（虚值）**时才赋值。Falsy 值（虚值）与 null 有所不同，因为 falsy 值（虚值）可以是任何一种值：undefined，null，空字符串 (双引号 ""、单引号’’、反引号 \`\`)，NaN，0。IE 浏览器中的 document.all，也算是一个。

语法

    x||=y

等同于

    x||(x=y)

在我们想要保留现有值（如果不存在）的情况下，这很有用，否则我们想为其分配默认值。例如，如果搜索请求中没有数据，我们希望将元素的内部 HTML 设置为默认值。否则，我们要显示现有列表。这样，我们避免了不必要的更新和任何副作用，例如解析，重新渲染，失去焦点等。我们可以简单地使用此运算符来使用 JavaScript 更新 HTML：

    document.getElementById('search').innerHTML||='<i>Nopostsfoundmatchingthissearch.</i>'

#### 四、逻辑与分配（&& =）

可能你已经猜到了，此逻辑赋值运算符仅在左侧为真时才赋值。因此：

    x&&=y

等同于

    x&&(x=y)

本次分享几个优雅的 JavaScript 运算符使用技巧，重点分享了可选链接运算符的使用，这样可以让我们不需要再编写大量我们例子中代码即可轻松访问嵌套属性。但是 IE 不支持它，因此，如果需要支持该版本或更旧版本的浏览器，则可能需要添加 Babel 插件。对于 Node.js，需要为此升级到 Node 14 LTS 版本，因为 12.x 不支持该版本。

## 写在最后

> `公众号`：`前端开发爱好者`专注分享`web`前端相关`技术文章`、`视频教程`资源、热点资讯等，如果喜欢我的分享，给 🐟🐟 点一个`赞`👍 或者 ➕`关注`都是对我最大的支持。

欢迎`长按图片加好友`，我会第一时间和你分享`前端行业趋势`，`面试资源`，`学习途径`等等。

![图片](https://cubox.pro/c/filters:no_upscale()?imageUrl=https%3A%2F%2Fmmbiz.qpic.cn%2Fmmbiz_jpg%2FkzFgl6ibibNKqeic8dhefYR9fYkKOd0B2Tb3WMgs3ib3ELXd6nRRjfrLsqfZMLZmice6YZg2HBHqh8aVBXUdjpDV95Q%2F640%3Fwx_fmt%3Djpeg%26wxfrom%3D5%26wx_lazy%3D1%26wx_co%3D1)添加好友备注【**进阶学习**】拉你进技术交流群

关注公众号后，在首页：

*   回复`面试题`，获取最新大厂面试资料。
    
*   回复`简历`，获取 3200 套 简历模板。
    
*   回复`React实战`，获取 React 最新实战教程。
    
*   回复`Vue实战`，获取 Vue 最新实战教程。
    
*   回复`ts`，获取 TypeScript 精讲课程。
    
*   回复`vite`，获取 Vite 精讲课程。
    
*   回复`uniapp`，获取 uniapp 精讲课程。
    
*   回复`js书籍`，获取 js 进阶 必看书籍。
    
*   回复`Node`，获取 Nodejs+koa2 实战教程。
    
*   回复`数据结构算法`，获取数据结构算法教程。
    
*   回复`架构师`，获取 架构师学习资源教程。
    
*   更多教程资源应有尽有，欢迎`关注获取`
    

[查看原网页: mp.weixin.qq.com](http://mp.weixin.qq.com/s?__biz=MzUzNTk3MjE2Ng==&mid=2247501534&idx=1&sn=9f880cd10d9ccc19d3147e8c418dc571&chksm=faffd6f7cd885fe1df636ebb3c655f704b12fae07d7dbf0256bf38322a3817c047e244d81486&mpshare=1&scene=1&srcid=1010vLpKUH2FmVvVciqMwfCJ&sharer_sharetime=1665413737217&sharer_shareid=b8d5da03cbe546fb54510ac993e581cf#rd)