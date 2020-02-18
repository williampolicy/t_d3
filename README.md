console.log()

### Learn d3

**D3.js** is a JavaScript library for manipulating documents based on data. **D3** helps you bring data to life using HTML, SVG, and CSS. D3’s emphasis on web standards gives you the full capabilities of modern browsers without tying yourself to a proprietary framework, combining powerful visualization components and a data-driven approach to DOM manipulation.



To link directly to the latest release, copy this snippet:

```
<script src="https://d3js.org/d3.v5.min.js"></script>
```

https://d3js.org/#introduction

​	

###Know javascripts





**D3** allows you to bind arbitrary data to a Document Object Model (DOM), and then apply data-driven transformations to the document. For example, you can use D3 to generate an HTML table from an array of numbers. Or, use the same data to create an interactive SVG bar chart with smooth transitions and interaction.

D3 is not a monolithic framework that seeks to provide every conceivable feature. Instead, D3 solves the crux of the problem: efficient manipulation of documents based on data. This avoids proprietary representation and affords extraordinary flexibility, exposing the full capabilities of web standards such as HTML, SVG, and CSS. With minimal overhead, D3 is extremely fast, supporting large datasets and dynamic behaviors for interaction and animation. D3’s functional style allows code reuse through a diverse collection of [official](https://github.com/d3/d3/blob/master/API.md) and [community-developed](https://www.npmjs.com/browse/keyword/d3-module) modules.



## Selections

Modifying documents using the [W3C DOM API](https://www.w3.org/DOM/DOMTR) is tedious: the method names are verbose, and the imperative approach requires manual iteration and bookkeeping of temporary state. For example, to change the text color of paragraph elements:

```python
var paragraphs = document.getElementsByTagName("p");
for (var i = 0; i < paragraphs.length; i++) {
  var paragraph = paragraphs.item(i);
  paragraph.style.setProperty("color", "blue", null);
}
```

D3 employs a declarative approach, operating on arbitrary sets of nodes called *selections*. For example, you can rewrite the above loop as:

```python
d3.selectAll("p").style("color", "blue");
```

Yet, you can still manipulate individual nodes as needed:

```python
d3.select("body").style("background-color", "black");
```

Selectors are defined by the [W3C Selectors API](https://www.w3.org/TR/selectors-api/) and supported natively by modern browsers. The above examples select nodes by tag name (`"p"` and `"body"`, respectively). Elements may be selected using a variety of predicates, including containment, attribute values, class and ID.

D3 provides numerous methods for mutating nodes: setting attributes or styles; registering event listeners; adding, removing or sorting nodes; and changing HTML or text content. These suffice for the vast majority of needs. Direct access to the underlying DOM is also possible, as each D3 selection is simply an array of nodes.



##  Dynamic Properties

Readers familiar with other DOM frameworks such as [jQuery](https://jquery.com/) should immediately recognize similarities with D3. Yet styles, attributes, and other properties can be specified as *functions of data* in D3, not just simple constants. Despite their apparent simplicity, these functions can be surprisingly powerful; the [d3.geoPath](https://github.com/d3/d3-geo/blob/master/README.md#geoPath) function, for example, projects [geographic coordinates](https://tools.ietf.org/html/rfc7946) into SVG [path data](https://www.w3.org/TR/SVG/paths.html#PathData). D3 provides many built-in reusable functions and function factories, such as [graphical primitives](https://github.com/d3/d3-shape) for area, line and pie charts.

For example, to randomly color paragraphs:

```js
d3.selectAll("p").style("color", function() {
  return "hsl(" + Math.random() * 360 + ",100%,50%)";
});
```

To alternate shades of gray for even and odd nodes:

```js
d3.selectAll("p").style("color", function(d, i) {
  return i % 2 ? "#fff" : "#eee";
});
```



Computed properties often refer to bound data. Data is specified as an array of values, and each value is passed as the first argument (`d`) to selection functions. With the default join-by-index, the first element in the data array is passed to the first node in the selection, the second element to the second node, and so on. For example, if you bind an array of numbers to paragraph elements, you can use these numbers to compute dynamic font sizes:

```js
d3.selectAll("p")
  .data([4, 8, 15, 16, 23, 42])
    .style("font-size", function(d) { return d + "px"; });
```

Once the data has been bound to the document, you can **omit the `data` operator**; D3 will retrieve the previously-bound data. This allows you to recompute properties without rebinding.

数据绑定到文档后，您可以省略`data`运算符；D3将检索先前绑定的数据。这使您无需重新绑定即可重新计算属性。

## [#](https://d3js.org/#enter-exit)Enter and Exit

Using D3’s *enter* and *exit* selections, you can create new nodes for incoming data and remove outgoing nodes that are no longer needed.

When data is bound to a selection, each element in the data array is paired with the corresponding node in the selection. If there are fewer nodes than data, the extra data elements form the enter selection, which you can instantiate by appending to the `enter` selection. For example:

```js
d3.select("body")
  .selectAll("p")
  .data([4, 8, 15, 16, 23, 42])
  .enter().append("p")
    .text(function(d) { return "I’m number " + d + "!"; });
```

Updating nodes are the default selection—the result of the `data` operator. Thus, if you forget about the enter and exit selections, you will automatically select only the elements for which there exists corresponding data. A common pattern is to break the initial selection into three parts: the updating nodes to modify, the entering nodes to add, and the exiting nodes to remove.

```js
// Update…
var p = d3.select("body")
  .selectAll("p")
  .data([4, 8, 15, 16, 23, 42])
    .text(function(d) { return d; });

// Enter…
p.enter().append("p")
    .text(function(d) { return d; });

// Exit…
p.exit().remove();
```

By handling these three cases separately, you specify precisely which operations run on which nodes. This improves performance and offers greater control over transitions. For example, with a bar chart you might initialize entering bars using the old scale, and then transition entering bars to the new scale along with the updating and exiting bars.

D3 lets you transform documents based on data; this includes both creating and destroying elements. D3 allows you to change an existing document in response to user interaction, animation over time, or even asynchronous notification from a third-party. A hybrid approach is even possible, where the document is initially rendered on the server, and updated on the client via D3.





##  Transformation, not Representation

D3 does not introduce a new visual representation. Unlike [Processing](https://processing.org/) or [Protovis](https://mbostock.github.io/protovis/), D3’s vocabulary of graphical marks comes directly from web standards: HTML, SVG, and CSS.

- For example, you can create SVG elements using D3 and style them with external stylesheets. You can use composite filter effects, dashed strokes and clipping. If browser vendors introduce new features tomorrow, you’ll be able to use them immediately—no toolkit update required. And, if you decide in the future to use a toolkit other than D3, you can take your knowledge of standards with you!
- Best of all, D3 is easy to debug using the browser’s built-in element inspector: the nodes that you manipulate with D3 are exactly those that the browser understands natively.

## Transitions

D3’s focus on transformation extends naturally to animated transitions. Transitions gradually interpolate styles and attributes over time. **Tweening can be controlled via easing functions such as “elastic”, “cubic-in-out” and “linear”**. D3’s interpolators support both primitives, such as numbers and numbers embedded within strings (font sizes, path data, *etc.*), and compound values. You can even extend D3’s interpolator registry to support complex properties and data structures.

For example, to fade the background of the page to black:

```js
d3.select("body").transition()
    .style("background-color", "black");
```

Or, to resize circles in a symbol map with a staggered delay:

```js
d3.selectAll("circle").transition()
    .duration(750)
    .delay(function(d, i) { return i * 10; })
    .attr("r", function(d) { return Math.sqrt(d * scale); });
```

By modifying only the attributes that actually change, D3 reduces overhead and allows greater graphical complexity at high frame rates. D3 also allows sequencing of complex transitions via events. And, you can still use CSS3 transitions; D3 does not replace the browser’s toolbox, but exposes it in a way that is easier to use.

```js
d3.select("body")
  .selectAll("p")
  .data([4, 8, 15, 16, 23, 42,100,21])
  .enter().append("p")
    .text(function(d) { return "This my number " + d + "!"; });
```

通过分别处理这三种情况，您可以精确地指定哪些操作在哪些节点上运行。这样可以提高性能并更好地控制过渡。例如，使用条形图，您可以使用旧的比例尺来初始化输入条形，然后将输入的条形图以及更新和退出条形过渡到新的比例尺。

D3使您可以基于数据转换文档；这包括创建和销毁元素。D3允许您更改现有文档，以响应用户交互，随时间变化的动画，甚至来自第三方的异步通知。甚至可以使用混合方法，其中文档最初在服务器上呈现，然后通过D3在客户端上更新。



### time 横坐标 。 纵坐标 Weight，找图。 

OK . 

Well done --> next：1 line

```
  var sumstat = d3.nest() // nest function allows to group the calculation per level of a factor
    .key(function(d) { return d.name;}) // find name
    .entries(data);
```

![image-20200216100634447](/Users/kang/Library/Application Support/typora-user-images/image-20200216100634447.png)

Well done --> next: data.structure. might be in one data or two data.set. 

just write down:

```
year,name,n,prop
1879,weightA,536
1880,weightA,636
1881,weightA,612
1882,weightA,912
1883,weightA,140
1884,weightA,700
1879,weightB,141
1880,weightB,241
1881,weightB,263
1881,weightB,109
1882,weightB,38
1883,weightB,140
1884,weightB,700
```

Change title. year - times



![image-20200216101653394](/Users/kang/Library/Application Support/typora-user-images/image-20200216101653394.png)

Well done --> next  

# NExt future , I will finish a Great Sensicity of People (might be 100000Point Sensitivity)





- Great,  I can  use it so set the weight. 



### renew js  in flask templates

61







Ultimately this is a frustrating browser cache issue, which can be solved by forcing the browser to do a "hard refresh", which is going to be a browser/OS dependent keystroke, but generally this works:

- Windows: Ctrl+F5
- Mac: Cmd+Shift+R
- Linux: Ctrl+Shift+R

There are other filename tricks one can use to avoid this issue (mentioned in comments of the OP). These are especially important in production where you have no control over browser behavior.

For non-Static Flask responses you can set the `cache_control.max_age` property, which should tell the browser when to expire the response if it is cached. For instance if you have a Flask XHR endpoint that returns JSON data you could do this:

```py
@app.route('/_get_ajax_data/')
def get_ajax_data():
    data = {"hello": "world"}
    response = jsonify(data)
    response.cache_control.max_age = 60 * 60 * 24  # 1 day (in seconds)
    return response
```

You typically can also set default values in your production web server configuration for specific resource types (e.g. CSS/JS/HTML/JSON/etc)

**Edit 4/1/2019** (unrelated to April Fools day)

- Mac / Safari keystroke now appears to be: Cmd+Opt+R (via comments, thanks!).
- See the new answer from @MarredCheese for a very elegant "filename trick" to force the browser to ignore cached copies for updated files.



well done , Next ->

## Error:  “GET /favicon.ico HTTP/1.1” 404 –


如果您的屏幕看起来如上图所示，则一切正常。 如果返回到终端，应该会看到一些附加输出：*在http://127.0.0.1:5000/上运行（按CTRL + C退出）127.0.0.1 – – [2018年2月11日14:03 ：00]“ GET / HTTP / 1.1” 200 -127.0.0.1 – – [11 / Feb / 2018 14:03:01]“ GET /favicon.ico HTTP / 1.1” 404 –

第一行是在告诉我们该应用程序正在运行并且正在侦听端口5000之前所看到的内容。第二行显示了我们的浏览器执行了HTTP get命令，而200则告诉我们它已成功。 最后一行是它尝试下载该图标的位置，但无法下载。 404错误意味着找不到favicon.ico。 这是有道理的，因为我们没有创建网站图标。 对于那些不知道的人，收藏夹图标是您的Web浏览器在网页的地址栏中显示的小图标。 并非所有的网络浏览器都显示它们。

If your screen looks like what is shown above, everything is working as expected. If you go back to your terminal, you should see some additional output:* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)127.0.0.1 – – [11/Feb/2018 14:03:00] “GET / HTTP/1.1” 200 -127.0.0.1 – – [11/Feb/2018 14:03:01] “GET /favicon.ico HTTP/1.1” 404 –

The first line is what we saw before telling us that the application is running and listening on port 5000. The next line is showing that our browser did an HTTP get command and the 200 tells us that it was successful. The last line is where it is trying to download the favicon, but it is not able to. The 404 error means that favicon.ico cannot be found. This makes sense because we did not create a favicon. For those who do not know, the favicon is the little icon that your web browser shows in the address bar for a web page. Not all web browsers even show them.



```
FLASK_APP=hello_world flask run --host 0.0.0.0
```

```
FLASK_APP=hello_world FLASK_DEBUG=1 flask run --host 0.0.0.0 --port 3000
```

```
FLASK_APP=hello_world FLASK_DEBUG=1 flask run --host 0.0.0.0 --port 3000
```

```
FLASK_APP=hello_world flask run --port 3000
```

https://blog.csdn.net/mydistance/article/details/84190749

当浏览器访问我们的服务器时，浏览器会默认请求项目根路径下的favicon.ico文件，根目录下没有这个文件，所以就报了这个错误。

INFO:werkzeug:127.0.0.1 - -  "GET /favicon.ico HTTP/1.1" 404 -

如何解决：

浏览器请求的是/favicon.ico，如图：



所以我们定义一个这样的路径，通过具体的方法实现就可以了，我们要做的是把favicon.ico文件，放到static文件夹下。

from flask import current_app

http://127.0.0.1:5000/favicon.ico

@news_blue.route('/favicon.ico')
def favicon():
    # 后端返回文件给前端（浏览器），send_static_file是Flask框架自带的函数
​    return current_app.send_static_file('static/favicon.ico')
那send_static_file是怎么实现的呢？

首先进入Flask类源码，可以看到Flask静态路由就是通过这个函数实现的

然后点进send_static_file看一下

 注意：

这里的'static/favicon.ico'中static是可有可无的，已经帮我们实现了

return current_app.send_static_file('static/favicon.ico')
不管写没写static，路由都是下图：



要显示ico图标，还要清空浏览器缓存，重新访问。
————————————————
版权声明：本文为CSDN博主「高岩_deal」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/mydistance/article/details/84190749