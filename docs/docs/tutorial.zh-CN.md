---
id: tutorial-zh-CN
title: 教程
prev: getting-started-zh-CN.html
next: thinking-in-react-zh-CN.html
---

我们将建立一个你可以放进博客的简单却真实的评论框，一个 Disqus、LiveFyre 或 Facebook comments 提供的实时评论的基础版本。
We'll be building a simple but realistic comments box that you can drop into a blog, a basic version of the realtime comments offered by Disqus, LiveFyre or Facebook comments.

我们将提供：
We'll provide:

* 一个所有评论的视图
* A view of all of the comments
* 一个用于提交评论的表单
* A form to submit a comment
* 为你提供制定后台的挂钩(Hooks)
* Hooks for you to provide a custom backend

同时也会有一些简洁的功能：
It'll also have a few neat features:

* **优化的评论：** 评论在它们保存到服务器之前就显示在列表里,所以会感觉很快。
* **Optimistic commenting:** comments appear in the list before they're saved on the server so it feels fast.
* **实时更新：** 其他用户的评论被实时浮现到评论中。
* **Live updates:** other users' comments are popped into the comment view in real time.
* **Markdown格式化：** 用户可以用Markdown格式化它们的文字。
* **Markdown formatting:** users can use Markdown to format their text.

### 想要跳过所有的内容，只查源代码？
### Want to skip all this and just see the source?

[都在 GitHub上.](https://github.com/reactjs/react-tutorial)
[It's all on GitHub.](https://github.com/reactjs/react-tutorial)

### 运行一个服务器

虽然这对于开始使用本教程并不是必须的，稍后我们将添加一个功能，对一个运行中的服务器请求 `POST`ing。如果这是你非常熟悉的事情并且你想创建你自己的服务器，那么请这样做吧。对于剩下的想尽可能关注于学习React而不用担心服务器端方面的人，我们已经用许多不同的语言编写了简单的服务器 - JavaScript (使用 Node.js), Python, Haskell, Ruby, Go, and PHP. 这些都在GitHub可以访问. 你可以 [查看源代码](https://github.com/reactjs/react-tutorial/) 或者 [下载 zip 文件](https://github.com/reactjs/react-tutorial/archive/master.zip) 来开始学习。
                                                      
                                                      
While it's not necessary to get started with this tutorial, later on we'll be adding functionality that requires `POST`ing to a running server. If this is something you are intimately familiar with and want to create your own server, please do. For the rest of you who might want to focus on learning about React without having to worry about the server-side aspects, we have written simple servers in a number of languages - JavaScript (using Node.js), Python, Haskell, Ruby, Go, and PHP. These are all available on GitHub. You can [view the source](https://github.com/reactjs/react-tutorial/) or [download a zip file](https://github.com/reactjs/react-tutorial/archive/master.zip) to get started.

开始使用下载的教程，只需开始编辑 `public/index.html` 。
To get started using the tutorial, just start editing `public/index.html`.

### 开始

对于此教程,我们将使用在CDN上的预构建 JavaScript 文件。在你最喜欢的编辑器里打开 `public/index.html` ，应该包含如下内容：
For this tutorial, we'll use prebuilt JavaScript files on a CDN. Open up `public/index.html` in your favorite editor, which should contain the following:

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello React</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/{{site.react_version}}/react.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/react/{{site.react_version}}/JSXTransformer.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
  </head>
  <body>
    <div id="content"></div>
    <script type="text/jsx">
      // Your code here
    </script>
  </body>
</html>
```

在本教程剩余的部分，我们将在此 script 标签中编写我们的 JavaScript 代码。在每次添加以后在你的浏览器里打开你的 index.html 文件来关注你的进度。
For the remainder of this tutorial, we'll be writing our JavaScript code in this script tag. Follow your progress by opening your index.html file in your browser after each addition.

> 注意：
>
> 我们在这里引入 jQuery 是因为我们想简化我们未来的 ajax 请求，但这对React的正常工作 **不是** 必要的。

> Note:
>
> We included jQuery here because we want to simplify the code of our future ajax calls, but it's **NOT** mandatory for React to work.

### 你的第一个组件

React 中全是关于模块化、可组装的组件。以我们的评论框为例，我们将有如下的组件结构：
### Your first component

React is all about modular, composable components. For our comment box example, we'll have the following component structure:

```
- CommentBox
  - CommentList
    - Comment
  - CommentForm
```

让我们构造 `CommentBox` 组件，仅是一个简单的 `<div>` ：
Let's build the `CommentBox` component, which is just a simple `<div>`:

```javascript
// tutorial1.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
React.render(
  <CommentBox />,
  document.getElementById('content')
);
```

注意原生的HTML元素以小写开头，而制定的 React 类以大写开头。
Note that native HTML element names start with a lowercase letter, while custom React class names begin with an uppercase letter.

#### JSX 语法

首先你会注意到你的 JavaScript 中 XML 式的语法。我们有一个简单的预编译器，将这种语法糖转换成单纯的 JavaScript ：
The first thing you'll notice is the XML-ish syntax in your JavaScript. We have a simple precompiler that translates the syntactic sugar to this plain JavaScript:

```javascript
// tutorial1-raw.js
var CommentBox = React.createClass({displayName: 'CommentBox',
  render: function() {
    return (
      React.createElement('div', {className: "commentBox"},
        "Hello, world! I am a CommentBox."
      )
    );
  }
});
React.render(
  React.createElement(CommentBox, null),
  document.getElementById('content')
);
```

它的使用是可选的，但是我们发现 JSX 语法比单纯的 JavaScript 更加容易使用。阅读更多关于[JSX 语法的文章](/react/docs/jsx-in-depth-zh-CN.html)。
Its use is optional but we've found JSX syntax easier to use than plain JavaScript. Read more on the [JSX Syntax article](/react/docs/jsx-in-depth.html).

#### What's going on

我们在一个 JavaScript 对象中传递一些方法到 `React.createClass()` 来创建一个新的React组件。这些方法中最重要的是 `render`，该方法返回一颗 React 组件树，这棵树最终将会渲染成 HTML。

这个 `<div>` 标签不是真实的DOM节点；他们是 React `div` 组件的实例化。你可以把这些看做是React知道如何处理的标记或者是一些数据 。React 是**安全的**。我们不生成 HTML 字符串，因此XSS防护是默认特性。

你没有必要返回基本的 HTML。你可以返回一个你（或者其他人）创建的组件树。这就使 React **组件化**：一个可维护前端的关键原则。

`React.render()` 实例化根组件，启动框架，注入标记到原始的 DOM 元素中，作为第二个参数提供。
We pass some methods in a JavaScript object to `React.createClass()` to create a new React component. The most important of these methods is called `render` which returns a tree of React components that will eventually render to HTML.

The `<div>` tags are not actual DOM nodes; they are instantiations of React `div` components. You can think of these as markers or pieces of data that React knows how to handle. React is **safe**. We are not generating HTML strings so XSS protection is the default.

You do not have to return basic HTML. You can return a tree of components that you (or someone else) built. This is what makes React **composable**: a key tenet of maintainable frontends.

`React.render()` instantiates the root component, starts the framework, and injects the markup into a raw DOM element, provided as the second argument.

## 组合组件
## Composing components

让我们为 `CommentList` 和 `CommentForm` 建造骨甲，它们将会，再一次的，是一些简单的 `<div>`。添加这两个组件到你的文件里，保持现存的 `CommentBox` 声明和 `React.render` 调用:
Let's build skeletons for `CommentList` and `CommentForm` which will, again, be simple `<div>`s. Add these two components to your file, keeping the existing `CommentBox` declaration and `React.render` call:

```javascript
// tutorial2.js
var CommentList = React.createClass({
  render: function() {
    return (
      <div className="commentList">
        Hello, world! I am a CommentList.
      </div>
    );
  }
});

var CommentForm = React.createClass({
  render: function() {
    return (
      <div className="commentForm">
        Hello, world! I am a CommentForm.
      </div>
    );
  }
});
```

接着，更新 `CommentBox` 以使用这些新的组件：
Next, update the `CommentBox` component to use these new components:

```javascript{6-8}
// tutorial3.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList />
        <CommentForm />
      </div>
    );
  }
});
```

注意我们是如何混合 HTML 标签和我们建立的组件。HTML 组件是正常的 React 组件，就和你定义的一样，只有一个区别。JSX 编译器会自动重写 HTML 标签为 `React.createElement(tagName)` 表达式，其它什么都不做。这是为了避免污染全局命名空间。
Notice how we're mixing HTML tags and components we've built. HTML components are regular React components, just like the ones you define, with one difference. The JSX compiler will automatically rewrite HTML tags to `React.createElement(tagName)` expressions and leave everything else alone. This is to prevent the pollution of the global namespace.

### 使用 props

让我们创建 `Comment` 组件，它将依赖于从父级传来的数据。从父级传来的数据在子组件里作为 '属性' 可供使用。 这些 '属性' 可以通过 `this.props` 访问。使用属性，我们将能读取从 `CommentList` 传递给 `Comment` 的数据，并且渲染一些标记：
Let's create the `Comment` component, which will depend on data passed in from its parent. Data passed in from a parent component is available as a 'property' on the child component. These 'properties' are accessed through `this.props`. Using props, we will be able to read the data passed to the `Comment` from the `CommentList`, and render some markup:

```javascript
// tutorial4.js
var Comment = React.createClass({
  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        {this.props.children}
      </div>
    );
  }
});
```

在 JSX 中,通过将 JavaScript 表达式放在大括号中（作为属性或者子节点）,你可以把文本或者 React 组件放置到树中。我们以 `this.props` 的 keys 访问传递给组件的命名属性，以 `this.props.children` 访问任何嵌套的元素。
By surrounding a JavaScript expression in braces inside JSX (as either an attribute or child), you can drop text or React components into the tree. We access named attributes passed to the component as keys on `this.props` and any nested elements as `this.props.children`.

### 组件的属性

既然我们已经定义了 `Comment` 组件，我们将要传递作者名和评论文字给它。这允许我们为每个评论重用相同的代码。现在让我们在我们的 `CommentList` 里添加一些评论。
Now that we have defined the `Comment` component, we will want to pass it the author name and comment text. This allows us to reuse the same code for each unique comment. Now let's add some comments within our `CommentList`:

```javascript{6-7}
// tutorial5.js
var CommentList = React.createClass({
  render: function() {
    return (
      <div className="commentList">
        <Comment author="Pete Hunt">This is one comment</Comment>
        <Comment author="Jordan Walke">This is *another* comment</Comment>
      </div>
    );
  }
});
```

注意，我们已经从 `CommentList`  组件传递了一些数据到 `Comment` 组件。例如，我们传递了 *Pete Hunt* （通过属性）和 *This is one comment* (通过 XML-风格的子节点)给第一个 `Comment`。如上面提到的那样， `Comment` 组件将会通过 `this.props.author` 和 `this.props.children` 访问 这些 '属性'。 
Note that we have passed some data from the parent `CommentList` component to the child `Comment` components. For example, we passed *Pete Hunt* (via an attribute) and *This is one comment* (via an XML-like child node) to the first `Comment`. As noted above, the `Comment` component will access these 'properties' through `this.props.author`, and `this.props.children`.

### 添加 Markdown

Markdown 是一种简单的内联格式化你的文字的方法。例如，用星号包围文本将会使其强调突出。
Markdown is a simple way to format your text inline. For example, surrounding text with asterisks will make it emphasized.

首先，添加第三方库 **marked** 到你的应用。这是一个JavaScript库，接受 Markdown 文本并且转换为原始的 HTML。这需要在你的头部有一个 script 标签（那个我们已经在 React 操场上包含了的标签）：
First, add the third-party library **marked** to your application. This is a JavaScript library which takes Markdown text and converts it to raw HTML. This requires a script tag in your head (which we have already included in the React playground):

```html{8}
<!-- index.html -->
<head>
  <meta charset="UTF-8" />
  <title>Hello React</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/{{site.react_version}}/react.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/{{site.react_version}}/JSXTransformer.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/marked/0.3.2/marked.min.js"></script>
</head>
```

然后，让我们转换评论文本为 Markdown 并输出它：
Next, let's convert the comment text to Markdown and output it:

```javascript{9}
// tutorial6.js
var Comment = React.createClass({
  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        {marked(this.props.children.toString())}
      </div>
    );
  }
});
```

我们在这里唯一做的就是调用 marked 库。我们需要把 从 React 的包裹文本来的 `this.props.children` 转换成 marked 能理解的原始字符串，所以我们显示地调用了`toString()`。

但是这里有一个问题！我们渲染的评论在浏览器里看起来像这样： "`<p>`This is `<em>`another`</em>` comment`</p>`" 。我们想让这些标签真正地渲染为 HTML。

那是 React 在保护你免受 [XSS 攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。有一个方法解决这个问题，但是框架会警告你别使用这种方法：
All we're doing here is calling the marked library. We need to convert `this.props.children` from React's wrapped text to a raw string that marked will understand so we explicitly call `toString()`.

But there's a problem! Our rendered comments look like this in the browser: "`<p>`This is `<em>`another`</em>` comment`</p>`". We want those tags to actually render as HTML.

That's React protecting you from an [XSS attack](https://en.wikipedia.org/wiki/Cross-site_scripting). There's a way to get around it but the framework warns you not to use it:

```javascript{4,10}
// tutorial7.js
var Comment = React.createClass({
  rawMarkup: function() {
    var rawMarkup = marked(this.props.children.toString(), {sanitize: true});
    return { __html: rawMarkup };
  },

  render: function() {
    return (
      <div className="comment">
        <h2 className="commentAuthor">
          {this.props.author}
        </h2>
        <span dangerouslySetInnerHTML={this.rawMarkup()} />
      </div>
    );
  }
});
```

这是一个特殊的 API，故意让插入原始的 HTML 变得困难，但是对于 marked 我们将利用这个后门。

**记住：** 使用这个功能你会依赖于 marked 是安全的。既然如此，我们传递 `sanitize: true` 告诉 marked escape 源码里任何的 HTML 标记，而不是直接不变的让他们通过。
This is a special API that intentionally makes it difficult to insert raw HTML, but for marked we'll take advantage of this backdoor.

**Remember:** by using this feature you're relying on marked to be secure. In this case, we pass `sanitize: true` which tells marked to escape any HTML markup in the source instead of passing it through unchanged.

### 挂钩数据模型 Hook up the data model

到目前为止我们已经完成了在源码里直接插入评论。作为替代，让我们渲染一团 JSON 数据到评论列表里。最终数据将会来自服务器，但是现在，写在你的源代码中：
So far we've been inserting the comments directly in the source code. Instead, let's render a blob of JSON data into the comment list. Eventually this will come from the server, but for now, write it in your source:

```javascript
// tutorial8.js
var data = [
  {author: "Pete Hunt", text: "This is one comment"},
  {author: "Jordan Walke", text: "This is *another* comment"}
];
```

我们需要以一种模块化的方式将这个数据传入到 `CommentList`。修改 `CommentBox` 和 `React.render()` 方法，以通过 props 传入数据到 `CommentList`：
We need to get this data into `CommentList` in a modular way. Modify `CommentBox` and the `React.render()` call to pass this data into the `CommentList` via props:

```javascript{7,15}
// tutorial9.js
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.props.data} />
        <CommentForm />
      </div>
    );
  }
});

React.render(
  <CommentBox data={data} />,
  document.getElementById('content')
);
```

既然现在数据在 `CommentList` 中可用了，让我们动态地渲染评论：
Now that the data is available in the `CommentList`, let's render the comments dynamically:

```javascript{4-10,13}
// tutorial10.js
var CommentList = React.createClass({
  render: function() {
    var commentNodes = this.props.data.map(function (comment) {
      return (
        <Comment author={comment.author}>
          {comment.text}
        </Comment>
      );
    });
    return (
      <div className="commentList">
        {commentNodes}
      </div>
    );
  }
});
```

就是这样！

### 从服务器获取数据

让我们用一些来自服务器的动态数据替换硬编码的数据。我们将移除数据的prop，用获取数据的URL来替换它：
Let's replace the hard-coded data with some dynamic data from the server. We will remove the data prop and replace it with a URL to fetch:

```javascript{3}
// tutorial11.js
React.render(
  <CommentBox url="comments.json" />,
  document.getElementById('content')
);
```

这个组件不同于和前面的组件，因为它必须重新渲染自己。该组件将不会有任何数据，直到请求从服务器返回，此时该组件或许需要渲染一些新的评论。
This component is different from the prior components because it will have to re-render itself. The component won't have any data until the request from the server comes back, at which point the component may need to render some new comments.

注意： 此代码在这个阶段不会工作。
Note: the code will not be working at this step.

### Reactive state

迄今为止,基于它自己的props，每个组件都渲染了自己一次。`props` 是不可变的：它们从父级传来并被父级“拥有”。为了实现交互，我们给组件引进了可变的 **state**。`this.state` 是组件私有的，可以通过调用 `this.setState()` 改变它。每当state更新，组件就重新渲染自己。
So far, based on its props, each component has rendered itself once. `props` are immutable: they are passed from the parent and are "owned" by the parent. To implement interactions, we introduce mutable **state** to the component. `this.state` is private to the component and can be changed by calling `this.setState()`. When the state updates, the component re-renders itself.

`render()` 方法被声明为一个带有 `this.props` 和 `this.state` 的函数。框架保证了 UI 总是与输入一致。
`render()` methods are written declaratively as functions of `this.props` and `this.state`. The framework guarantees the UI is always consistent with the inputs.

当服务器获取数据时，我们将会改变我们已有的评论数据。让我们给 `CommentBox` 组件添加一组评论数据作为它的状态：
When the server fetches data, we will be changing the comment data we have. Let's add an array of comment data to the `CommentBox` component as its state:

```javascript{3-5,10}
// tutorial12.js
var CommentBox = React.createClass({
  getInitialState: function() {
    return {data: []};
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});
```

`getInitialState()` 在生命周期里只执行一次，并设置组件的初始状态。
`getInitialState()` executes exactly once during the lifecycle of the component and sets up the initial state of the component.

#### Updating state
When the component is first created, we want to GET some JSON from the server and update the state to reflect the latest data. In a real application this would be a dynamic endpoint, but for this example we will keep things simple by creating a static JSON file `public/comments.json` containing the array of comments:

```javascript
// tutorial13.json
[
  {"author": "Pete Hunt", "text": "This is one comment"},
  {"author": "Jordan Walke", "text": "This is *another* comment"}
]
```

We'll use jQuery to help make an asynchronous request to the server.

Note: because this is becoming an AJAX application you'll need to develop your app using a web server rather than as a file sitting on your file system. [As mentioned above](#running-a-server), we have provided several servers you can use [on GitHub](https://github.com/reactjs/react-tutorial/). They provide the functionality you need for the rest of this tutorial.

```javascript{6-18}
// tutorial13.js
var CommentBox = React.createClass({
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});
```

Here, `componentDidMount` is a method called automatically by React when a component is rendered. The key to dynamic updates is the call to `this.setState()`. We replace the old array of comments with the new one from the server and the UI automatically updates itself. Because of this reactivity, it is only a minor change to add live updates. We will use simple polling here but you could easily use WebSockets or other technologies.

```javascript{3,15,20-21,35}
// tutorial14.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm />
      </div>
    );
  }
});

React.render(
  <CommentBox url="comments.json" pollInterval={2000} />,
  document.getElementById('content')
);

```

All we have done here is move the AJAX call to a separate method and call it when the component is first loaded and every 2 seconds after that. Try running this in your browser and changing the `comments.json` file; within 2 seconds, the changes will show!

### Adding new comments

Now it's time to build the form. Our `CommentForm` component should ask the user for their name and comment text and send a request to the server to save the comment.

```javascript{5-9}
// tutorial15.js
var CommentForm = React.createClass({
  render: function() {
    return (
      <form className="commentForm">
        <input type="text" placeholder="Your name" />
        <input type="text" placeholder="Say something..." />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

Let's make the form interactive. When the user submits the form, we should clear it, submit a request to the server, and refresh the list of comments. To start, let's listen for the form's submit event and clear it.

```javascript{3-14,16-19}
// tutorial16.js
var CommentForm = React.createClass({
  handleSubmit: function(e) {
    e.preventDefault();
    var author = React.findDOMNode(this.refs.author).value.trim();
    var text = React.findDOMNode(this.refs.text).value.trim();
    if (!text || !author) {
      return;
    }
    // TODO: send request to the server
    React.findDOMNode(this.refs.author).value = '';
    React.findDOMNode(this.refs.text).value = '';
    return;
  },
  render: function() {
    return (
      <form className="commentForm" onSubmit={this.handleSubmit}>
        <input type="text" placeholder="Your name" ref="author" />
        <input type="text" placeholder="Say something..." ref="text" />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

##### Events

React attaches event handlers to components using a camelCase naming convention. We attach an `onSubmit` handler to the form that clears the form fields when the form is submitted with valid input.

Call `preventDefault()` on the event to prevent the browser's default action of submitting the form.

##### Refs

We use the `ref` attribute to assign a name to a child component and `this.refs` to reference the component. We can call `React.findDOMNode(component)` on a component to get the native browser DOM element.

##### Callbacks as props

When a user submits a comment, we will need to refresh the list of comments to include the new one. It makes sense to do all of this logic in `CommentBox` since `CommentBox` owns the state that represents the list of comments.

We need to pass data from the child component back up to its parent. We do this in our parent's `render` method by passing a new callback (`handleCommentSubmit`) into the child, binding it to the child's `onCommentSubmit` event. Whenever the event is triggered, the callback will be invoked:

```javascript{16-18,31}
// tutorial17.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    // TODO: submit to the server and refresh the list
  },
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

Let's call the callback from the `CommentForm` when the user submits the form:

```javascript{10}
// tutorial18.js
var CommentForm = React.createClass({
  handleSubmit: function(e) {
    e.preventDefault();
    var author = React.findDOMNode(this.refs.author).value.trim();
    var text = React.findDOMNode(this.refs.text).value.trim();
    if (!text || !author) {
      return;
    }
    this.props.onCommentSubmit({author: author, text: text});
    React.findDOMNode(this.refs.author).value = '';
    React.findDOMNode(this.refs.text).value = '';
    return;
  },
  render: function() {
    return (
      <form className="commentForm" onSubmit={this.handleSubmit}>
        <input type="text" placeholder="Your name" ref="author" />
        <input type="text" placeholder="Say something..." ref="text" />
        <input type="submit" value="Post" />
      </form>
    );
  }
});
```

Now that the callbacks are in place, all we have to do is submit to the server and refresh the list:

```javascript{17-28}
// tutorial19.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      type: 'POST',
      data: comment,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

### Optimization: optimistic updates

Our application is now feature complete but it feels slow to have to wait for the request to complete before your comment appears in the list. We can optimistically add this comment to the list to make the app feel faster.

```javascript{17-19}
// tutorial20.js
var CommentBox = React.createClass({
  loadCommentsFromServer: function() {
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      cache: false,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  handleCommentSubmit: function(comment) {
    var comments = this.state.data;
    var newComments = comments.concat([comment]);
    this.setState({data: newComments});
    $.ajax({
      url: this.props.url,
      dataType: 'json',
      type: 'POST',
      data: comment,
      success: function(data) {
        this.setState({data: data});
      }.bind(this),
      error: function(xhr, status, err) {
        console.error(this.props.url, status, err.toString());
      }.bind(this)
    });
  },
  getInitialState: function() {
    return {data: []};
  },
  componentDidMount: function() {
    this.loadCommentsFromServer();
    setInterval(this.loadCommentsFromServer, this.props.pollInterval);
  },
  render: function() {
    return (
      <div className="commentBox">
        <h1>Comments</h1>
        <CommentList data={this.state.data} />
        <CommentForm onCommentSubmit={this.handleCommentSubmit} />
      </div>
    );
  }
});
```

### Congrats!

You have just built a comment box in a few simple steps. Learn more about [why to use React](/react/docs/why-react.html), or dive into the [API reference](/react/docs/top-level-api.html) and start hacking! Good luck!
