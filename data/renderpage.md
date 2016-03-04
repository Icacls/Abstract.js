#快速上手
##渲染页面
我们将学习如何使用RenderModel去渲染页面模块

<img src="image/header.jpg" alt="" />


<a href='http://runjs.cn/code/pdl9ae3u' target="_blank">
可以看这个实例
</a>

##静态页面
首先，我们需要使用html和css写出我们的静态页面。HTML结构如下

<pre class="brush: xml">
<div class="header">
    <div class="left logo"></div>
    <div class="right intro">
        <h2>QQ</h2>
        <div class="subtitle subintro">Feeds 50, 000 Follow 930,000</div>
        <div class="level">
            <span class="icon levelicon"></span>
            <span class="levelRect" style="width: 93%"></span>
            <span class="levelnumber">0/15</span>
        </div>

        <div class="sign">sign</div>
    </div>
</div>
</pre>

##提取模板
第二步，我们需要将模板从上面的静态页面中抽离出来

HTML结构将会如下

<pre class="brush: xml">
<div class="header">
</div>
</pre>

模板文件如下（Abstract使用SodaRender进行渲染 <a href='http://alloyteam.github.io/SodaRender/'>sodaRender</a>）
<pre class="brush: xml">
    <div class="left logo"></div>
    <div class="right intro">
        <h2>{{title}}</h2>
        <div class="subtitle subintro">Feeds {{feeds | number}} Follow {{follow | number}}</div>
        <div class="level">
            <span class="icon levelicon"></span>
            <span class="levelRect" style="width: {{width}}%"></span>
            <span class="levelnumber">{{currLevel}}/{{totalLevel}}</span>
        </div>

        <div class="sign">sign</div>
    </div>

</pre>

##构建逻辑
因为这仅仅是一个普通的渲染模块，我们使用RenderModel进行渲染

<pre class="brush:js">
var header = new RenderModel({
    el: ".header",
    data: {
        title: "QQ",
        feeds: 50000,
        follow: 930000,
        width: 93,
        currLevel: 0,
        totalLevel: 15
    },
    
    processData: function(data){
    },

    tmpl: tmplStr,

    complete: function(data){
    }
});

header.rock();
</pre>

配置项解释

el: 渲染到的元素

data: 如果给了，将使用它做为数据来渲染页面

processData: 加工数据

tmpl: 模板字符串

complete: 函数，渲染完成做一些事情

模板使用了SodaRender引擎的filter "number", 我们定义sodaRender filter如下
<pre class="brush:js">
sodaFilter("number", function(input){
      // ... some code here to format number
});
</pre>

可以看到页面就渲染出来了



## 从Server获取数据
如果你需要从Server拿数据，只需要配置一个url参数和param参数就可以了
<pre class="brush:js">
var header = new RenderModel({
    el: ".header",

    url: "/cgi-bin/get_header",
    param: {
        id: 1
    },

    processData: function(data){
    },

    tmpl: tmplStr,

    complete: function(data){
    }
});

header.rock();
</pre>

param也可以是函数
<pre class="brush:js">
var header = new RenderModel({
    el: ".header",

    url: "/cgi-bin/get_header",
    param: function(){
        return {
            id: 1
        };
    },

    processData: function(data){
    },

    tmpl: tmplStr,

    complete: function(data){
    }
});

header.rock();
</pre>


注意： RenderModel会把数据缓存到localStorage, 下将RenderModel第一次渲染将会使用缓存数据进行渲染。同时也会发cgi请求server的数据。如果数据回来了就会进行第二次渲染，这样用户可以很快的看到数据。

如果你不希望使用这样的功能，你可以使用"noCache" 这个配置项越过第一次的缓存渲染


## 刷新模块
如果你想刷新这个模块，只需要调用refresh方法即可

<pre class="brush:js">
header.refresh();
</pre>

## 更多的事情
有更多的配置项可供使用。参加Doc