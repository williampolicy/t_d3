
#Very Great Learning File  like a 教科书
https://alignedleft.com/tutorials/d3/adding-elements


One of your first steps will be to use D3 to create a new DOM element. Typically, this will be an SVG object for rendering a data visualization, but we’ll start simple, and just create a p paragraph element.


```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <title>D3 Test</title>
        <script type="text/javascript" src="d3/d3.v3.js"></script>
    </head>
    <body>
        <script type="text/javascript">
            // Your beautiful D3 code will go here
        </script>
    </body>
</html>
```


Back in your HTML, replace the comment between the script tags with:

```
d3.select("body").append("p").text("New paragraph!");
```



See the difference? Now in the DOM, there is a new paragraph element that was generated on-the-fly! This may not be exciting yet, but you will soon use a similar technique to dynamically generate tens or hundreds of elements, each one corresponding to a piece of your data set.

Let’s walk through what just happened. In sequence, we:



Invoked D3's select method, which selects a single element from the DOM using CSS selector syntax. (We selected the body.)
Created a new p element and appended that to the end of our selection, meaning just before the closing </body> tag in this case.
Set the text content of that new, empty paragraph to “New paragraph!”
All of those crazy dots are just part of D3’s chain syntax.


Going Chainless
Our sample code could be rewritten without chain syntax as:

```
var body = d3.select("body");
var p = body.append("p");
p.text("New paragraph!");
```


Binding data

What is binding, and why would I want to do it to my data?

Data visualization is a process of mapping data to visuals. Data in, visual properties out. Maybe bigger numbers make taller bars, or special categories trigger brighter colors. The mapping rules are up to you.

With D3, we bind our data input values to elements in the DOM. Binding is like “attaching” or associating data to specific elements, so that later you can reference those values to apply mapping rules. Without the binding step, we have a bunch of data-less, un-mappable DOM elements. No one wants that.



In a Bind
We use D3’s selection.data() method to bind data to DOM elements. But there are two things we need in place first, before we can bind data:

The data
A selection of DOM elements
Let’s tackle these one at a time.


```
DOM
The Document Object Model refers to the hierarchical structure of HTML. Each bracketed tag is an element, and we refer to elements’ relative relationships to each other in human terms: parent, child, sibling, ancestor, and descendant. In the HTML above, body is the parent element to both of its children, h1 and p (which are siblings to each other). All elements on the page are descendants of html.

Web browsers parse the DOM in order to make sense of a page’s content.
```


hierarchical structure of HTML


Data
D3 is smart about handling different kinds of data, so it will accept practically any array of numbers, strings, or objects (themselves containing other arrays or key/value pairs). It can handle JSON (and GeoJSON) gracefully, and even has a built-in method to help you load in CSV files.

But to keep things simple, for now we will start with a boring array of numbers. Here is our sample data set:


```
var dataset = [ 5, 10, 15, 20, 25 ];
```

Please Make Your Selection

First, you need to decide what to select. That is, what elements will your data be associated with? Again, let’s keep it super simple and say that we want to make a new paragraph for each value in the data set. So you might imagine something like this would be helpful

```js
d3.select("body").selectAll("p")
```

and you’d be right, but there’s a catch: The paragraphs we want to select *don’t exist yet*. **And this gets at one of the most common points of confusion with D3: How can we select elements that don’t yet exist?** Bear with me, as the answer may require bending your mind a bit.忍受我，因为答案可能需要一点心思。

**The answer lies with `enter()`, a truly magical method. Here’s our final code for this example, which I’ll explain:**

```js
d3.select("body").selectAll("p")
    .data(dataset)
    .enter()
    .append("p")
    .text("New paragraph!");
```

[Now look at what that code does on this demo page.](https://alignedleft.com/tutorials/d3/binding-data/index.html) You see five new paragraphs, each with the same content. Here’s what’s happening.

`d3.select("body")` — Finds the `body` in the DOM and hands a reference off to the next step in the chain.

`.selectAll("p")` — Selects all paragraphs in the DOM. **Since none exist yet, this returns an empty selection.** Think of this empty selection as representing the paragraphs that ***will soon exist*.**

`.data(dataset)` — **Counts and parses our data values.** **There are five values in our data set, so everything past this point is executed five times, once for each value.**

`.enter()` — **To create new, data-bound elements, you must use `enter()`.** This method looks at the DOM, and then at the data being handed to it. If there are more data values than corresponding DOM elements, then **`enter()` *creates a new placeholder element* on which you may work your magic.** It then hands off a reference to this new placeholder to the next step in the chain.

`.append("p")` — **Takes the placeholder selection created by `enter()`** and inserts a `p` element into the DOM. Hooray! Then it hands off a reference to the element it just created to the next step in the chain.

万岁！ 然后，它将对刚创建的元素的引用交给链中的下一步。

`.text("New paragraph!")` — Takes the reference to the newly created `p` and inserts a text value.

![image-binding_data](/Users/kang/Code4policy/Development/test_code/t_d3/t_d3_turorials_alignedleft/note/img/image-binding_data.png)



See it? Do you see it? I can barely contain myself. There it is:



Our first data value, the number `5`, is showing up under the first paragraph’s `__data__` attribute. Click into the other paragraph elements, and you’ll see they also contain `__data__` values: 10, 15, 20, and 25, just as we specified.

You see, when D3 binds data to an element, that data doesn’t exist in the DOM, **but it does exist in memory as a `__data__` attribute of that element.** And the console is where you can go to confirm whether or not your data was bound as expected.

```console
console.log(d3.selectAll("p"))
```

![data_99](/Users/kang/Code4policy/Development/test_code/t_d3/t_d3_turorials_alignedleft/note/img/data_99.png)



```html
 <body>
        <script type="text/javascript">
            // Your beautiful D3 code will go here
            // d3.select("body").append("p").text("append to body, with word,first p");

            var dataset = [ 99, 10, 15, 20, 25 ];

            d3.select("body").selectAll("p")
            .data(dataset)
            .enter()
            .append("p")
            .text("New paragraph!");

        </script>
    </body>
```



Can you see 99?  Great !  Wow! 

 Hooray! Then it hands off a reference to the element it just created to the next step in the chain.



## Using your data

Last updated 2018 December 27

These tutorials address an older version of D3 (3.x) and will no longer be updated. See my book [*Interactive Data Visualization for the Web, 2nd Ed.*](https://alignedleft.com/work/d3-book-2e) to learn all about the current version of D3 (4.x).

Once you’ve loaded in your data and bound it to newly created elements in the DOM, how can you *use* it? Here’s our code from last time:

```
var dataset = [ 5, 10, 15, 20, 25 ];

d3.select("body").selectAll("p")
    .data(dataset)
    .enter()
    .append("p")
    .text("New paragraph!");
```

Let’s change the last line to:

```
    .text(function(d) { return d; });
```

[Check out what the new code does on this demo page.](https://alignedleft.com/tutorials/d3/using-your-data/1.html)

Whoa! We used our data to populate the contents of each paragraph, **all thanks to the magic of the `data()` method.** You see, when chaining methods together, anytime after you call `data()`, you can create an anonymous function that accepts `d` as input. **The magical `data()` method ensures that `d` is set to the corresponding value in your original data set, given the current element at hand.**

The value of “the current element” changes over time as D3 loops through each element. For example, when looping through the third time, our code creates the third `p` tag, and `d` will correspond to the third value in our data set (or `dataset[2]`). So the third paragraph gets text content of “15”.




705/5000

哇！ 我们使用我们的数据来填充每个段落的内容，**这全都归功于data（）方法的神奇之处。**您看到，将方法链接在一起时，只要您调用`data（）`之后的任何时间，您都可以 创建一个接受`d`作为输入的匿名函数。 **神奇的data（）方法可确保将d设置为原始数据集中相应的值（考虑到当前元素）。

当D3遍历每个元素时，“当前元素”的值会随时间变化。 例如，当第三次循环时，我们的代码将创建第三个`p`标签，而`d`将对应于数据集中的第三个值（或dataset [2]）。 因此，第三段的文字内容为“ 15”。



就是一个模板的遍历填充！！！

## High-functioning

In case you’re new to writing your own functions (a.k.a. methods), the basic structure of a function definition is:

```js
function(input_value) {
    //Calculate something here
    return output_value;
}
```

The function we used above is dead simple, nothing fancy

```
function(d) {
    return d;
}
```

and it’s wrapped within D3’s `text()` function, so whatever our function returns is handed off to `text()`.

被包装在D3的`text（）`函数中，因此我们函数的结果都将移交给`text（）`。

```
.text(function(d) {
    return d;
});
```

**But we can (and will) get much fancier, because you can customize these functions however you want.** Yes, it’s the pleasure and pain of writing your own JavaScript. We can define our own custom functions however we want. Maybe you’d like to add some extra text, which [produces this result](https://alignedleft.com/tutorials/d3/using-your-data/2.html).

```
.text(function(d) {
    return "I can count up to " + d;
});
```



handoff to text 传递

## Data Wants to be Held

**You may be wondering why you have to write out `function(d)...` instead of just `d` on its own. For example, this won’t work:**

```
.text("I can count up to " + d);
```

**In this context, without wrapping `d` in an anonymous function, `d` has no value. Think of `d` as a lonely little placeholder value that just needs a warm, containing hug from a kind, caring function’s parantheses.** (Extending this metaphor further, yes, it is creepy that the hug is being given by an *anonymous* function — stranger danger! — but that only confuses matters.)

Here is `d` being gently and appropriately held by a function:

```
.text(function(d) {  // <-- Note tender embrace at left
    return "I can count up to " + d;
});
```



The reason for this syntax is that `.text()`, `attr()`, **and many other D3 methods take a function as an argument.** For example, `text()` can take either simply a static string of text as an argument:

D3的许多其他方法都将函数作为参数。例如，`text（）`可以简单地将静态文本字符串作为参数：

```
.text("someString")
```

…*or* **the result of a function**:

```
.text(someFunction())
```

…*or* **an anonymous function itself can be the argument**, such as when you write:

```
.text(function(d) {
    return d;
})
```

Above, you are defining an anonymous function. If D3 sees a function there, **it will *call* that function, while handing off the current datum `d` as the function’s argument.** **Without the function in place, D3 can’t know to whom it should hand off the argument `d`.**

**At first, this may seem silly and like a lot of extra work to just get at `d`, but the value of this approach will become clear as we work on more complex pieces.**



## Beyond Text

Things get a lot more interesting when we explore D3’s other methods, like `attr()` and `style()`, which allow us to set HTML attributes and CSS properties on selections, respectively.

For example, adding one more line to our code [produces this result](https://alignedleft.com/tutorials/d3/using-your-data/3.html).

```js
.style("color", "red");
```

All the text is now red; **big deal.** But we could use a custom function to make the text red only if the current datum exceeds a certain threshold. So we revise that last line to use a function:

```js
.style("color", function(d) {
    if (d > 15) {   //Threshold of 15
        return "red";
    } else {
        return "black";
    }
});
```

[See that code in action.](https://alignedleft.com/tutorials/d3/using-your-data/4.html) Notice how the first three lines are black, but once `d` exceeds the arbitrary threshold of 15, the text turns red.

In the next tutorial, we’ll use `attr()` and `style()` to manipulate `div`s, generating a simple bar chart — our first visualization!

```js
            var dataset = [ 99, 10, 15, 20, 25 ];

            d3.select("body").selectAll("p")
            .data(dataset)
            .enter()
            .append("p")
            // .style("color", "red")
            // .text("I try to say d in text"+d);  without wrapping `d` in an anonymous function, `d` has no value. 
            .style("color", function(d) {
                                        if (d > 15) {   //Threshold of 15
                                                        return "red";
                                                    } 
                                         else       {
                                                        return "black";
                                                    }
                                        })
            .text(function(d) {
                                 return "Grace Kang can count up to " + d;
                              });
```

well done --> next



## Drawing divs

These tutorials address an older version of D3 (3.x) and will no longer be updated. See my book [*Interactive Data Visualization for the Web, 2nd Ed.*](https://alignedleft.com/work/d3-book-2e) to learn all about the current version of D3 (4.x).

It’s time to start drawing with data.

Let’s continue working with our simple data set from last time:

```
var dataset = [ 5, 10, 15, 20, 25 ];
```

We’ll use this to generate a super-simple bar chart. Bar charts are essentially just rectangles, and an HTML `` is the easiest way to draw a rectangle. (Then again, to a web browser, *everything* is a rectangle, so you could easily adapt this example to use `span`s or whatever element you prefer.)

This `div` could work well as a data bar:

```
<div style="display: inline-block;
            width: 20px;
            height: 75px;
            background-color: teal;"></div>
```

(Among web standards folks, this is a semantic no-no. Normally, one shouldn’t use an empty `div` for purely visual effect, but coding tutorials are notable exceptions.)

Because this is a `div`, its `width` and `height` are set with CSS styles. Each bar in our chart will share the same display properties (except for `height`), so I’ll put those shared styles into a class called `bar`:

因为这是一个div，所以它的宽度和高度是使用CSS样式设置的。图表中的每个条形图都会共享相同的显示属性（高度除外），因此，我将这些共享的样式放入名为bar的类中：

```
div.bar {
    display: inline-block;
    width: 20px;
    height: 75px;   /* We'll override this later */
    background-color: teal;
}
```

Now each `div` needs to be assigned the `bar` class, so our new CSS rule will apply. If you were writing the HTML code by hand, you would write:

```
<div class="bar"></div>
```

Using D3, to add a class to an element, we use the `selection.attr()` method. It’s important to understand the difference between `attr()` and its close cousin, `style()`.



## Setting Attributes

[`attr()`](https://github.com/mbostock/d3/wiki/Selections#wiki-attr) is used to set an HTML attribute and its value on an element. An HTML attribute is any property/value pair that you could include between an element’s `<>` brackets. For example, these HTML elements

设定属性
attr（）用于在元素上设置HTML属性及其值。 HTML属性是您可以在元素的<>括号之间包含的任何属性/值对。例如，这些HTML元素



```js
<p class="caption">
<select id="country">
<img src="logo.png" width="100px" alt="Logo" />
```

contain a total of five attributes (and corresponding values), all of which could be set with `attr()`:

```js
class   |   caption
id      |   country
src     |   logo.png
width   |   100px
alt     |   Logo
```



To give our `div`s a class of `bar`, we can use:

```js
.attr("class", "bar")
```



## A Note on Classes

Note that an element’s *class* is stored as an HTML attribute. The class, in turn, is used to reference a CSS style rule. This may cause some confusion because there is a difference between setting a *class* (from which styles are inferred) and applying a *style* directly to an element. You can do both with D3. Although you should use whatever approach makes the most sense to you, I recommend **using *classes* for properties that are shared by multiple elements**, and applying *style* rules directly only when deviating from the norm. (In fact, that’s what we’ll do in just a moment.)

I also want to briefly mention another D3 method, `classed()`, which can be used to quickly apply or remove classes from elements. The line of code above could be rewritten as:

请注意，元素的* class *存储为HTML属性。反过来，该类用于引用CSS样式规则。这可能会引起一些混乱，因为设置* class *（从中推断出样式）与直接将* style *应用于元素之间存在差异。您可以同时使用D3。尽管您应该使用最适合您的方法，但我建议对由多个元素共享的属性使用* classes *，并且仅在偏离规范时才直接应用* style *规则。 （实际上，这就是我们稍后要做的。）

**我还想简短地提到另一个D3方法“ classed（）”，该方法可用于快速从元素中应用或删除类。上面的代码行可以重写为：**

```js
.classed("bar", true)
```



## Back to the Bars

Putting it all together with our data set, here is the complete D3 code so far:

```js
var dataset = [ 5, 10, 15, 20, 25 ];

d3.select("body").selectAll("div")
    .data(dataset)
    .enter()
    .append("div")
    .attr("class", "bar");
```

[See this demo page with that code.](https://alignedleft.com/tutorials/d3/drawing-divs/1.html) Make sure to view the source, and open your web inspector to see what’s going on. You should see five vertical bars, one generated for each point in our data set, although with no space between them, they look like one big rectangle.





## Setting Styles

The [`style()`](https://github.com/mbostock/d3/wiki/Selections#wiki-style) method is used to apply a CSS property and value directly to an HTML element. This is the equivalent of including CSS rules within a `style` attribute right in your HTML, as in:

```
<div style="height: 75px;"></div>
```

In a bar chart, the height of each bar must be a function of the corresponding data value. So let’s add this to the end of our D3 code:

```js
.style("height", function(d) {
    return d + "px";
});
```

[See this demo page with that code.](https://alignedleft.com/tutorials/d3/drawing-divs/2.html) You should see a very small bar chart!

When D3 **loops through** each data point, the value of `d` will be set to that of the corresponding data point. So we are setting a `height` value of `d` (the current data value) plus `px` (to specify the units are pixels). **The resulting heights will be 5px, 10px, 15px, 20px, and 25px.**

This looks a little bit silly, so let’s make those bars taller

```
.style("height", function(d) {
    var barHeight = d * 5;  //Scale up by factor of 5
    return barHeight + "px";
});
```

and add some space to the right of each bar, to space things out:

```
margin-right: 2px;
```



```js

            d3.select("body").selectAll("div")
                .data(dataset)
                .enter()
                .append("div")
                .attr("class", "bar")
                // .style("height", function(d) {
                //                                     return d + "px";
                //                                 });

                .style("height", function(d) {
                                                var barHeight = d * 5;  //Scale up by factor of 5
                                                    return barHeight + "px";
                                            });

```



```html
    <style type="text/css">
            div.bar {
                display: inline-block;
                width: 20px;
                height: 75px;   /* We'll override this later */
                background-color: teal;
                margin-right: 20px;
    </style>

```

![Screen Shot 2020-02-18 at 6.35.07 PM](/Users/kang/Code4policy/Development/test_code/t_d3/t_d3_turorials_alignedleft/note/img/Screen Shot 2020-02-18 at 6.35.07 PM.png)



well done ->





## The power of data()

These tutorials address an older version of D3 (3.x) and will no longer be updated. See my book [*Interactive Data Visualization for the Web, 2nd Ed.*](https://alignedleft.com/work/d3-book-2e) to learn all about the current version of D3 (4.x).

We left off with [a simple bar chart](https://alignedleft.com/tutorials/d3/the-power-of-data/1.html), drawn with `div`s and generated from our simple data set.

```
var dataset = [ 5, 10, 15, 20, 25 ];
```

![Simple bar chart](https://alignedleft.com/content/03-tutorials/01-d3/90-the-power-of-data/1.png)

This is great, but real-world data never looks like this. Let’s [modify our data](https://alignedleft.com/tutorials/d3/the-power-of-data/2.html).

```
var dataset = [ 25, 7, 5, 26, 11 ];
```

![Bar chart](https://alignedleft.com/content/03-tutorials/01-d3/90-the-power-of-data/2.png)

We’re not limited to five data points, of course. Let’s [add more](https://alignedleft.com/tutorials/d3/the-power-of-data/3.html)!

```
var dataset = [ 25, 7, 5, 26, 11, 8, 25, 14, 23, 19,
                14, 11, 22, 29, 11, 13, 12, 17, 18, 10,
                24, 18, 25, 9, 3 ];
```

![Bar chart](https://alignedleft.com/content/03-tutorials/01-d3/90-the-power-of-data/3.png)

25 data points instead of five! How does D3 automatically expand our chart as needed?

```
d3.select("body").selectAll("div")
    .data(dataset)  // <-- The answer is here!
    .enter()
    .append("div")
    .attr("class", "bar")
    .style("height", function(d) {
        var barHeight = d * 5;
        return barHeight + "px";
    });
```

Give `data()` ten values, and it will loop through ten times. Give it one million values, and it will loop through one million times. (Just be patient.)

That is the power of `data()` — being smart enough to loop through the full length of whatever data set you throw at it, executing each method below it in the chain, while updating the context in which each method operates, so `d` always refers to the current datum at that point in the loop.

That may be a mouthful, and if it all doesn’t make sense yet, it will soon. I encourage you to save the source code from the sample HTML pages above, tweak the `dataset` values, and note how the bar chart changes.

Remember, the *data* is driving the visualization — not the other way around.

## Random Data

Sometimes it’s fun to generate random data values, for testing purposes or pure geekiness. [That’s just what I’ve done here.](https://alignedleft.com/tutorials/d3/the-power-of-data/4.html) Notice how each time you reload the page, the bars render differently.

![Bar chart with random values](https://alignedleft.com/content/03-tutorials/01-d3/90-the-power-of-data/4.png)

![Bar chart with random values](https://alignedleft.com/content/03-tutorials/01-d3/90-the-power-of-data/5.png)

![Bar chart with random values](https://alignedleft.com/content/03-tutorials/01-d3/90-the-power-of-data/6.png)

View the source, and you’ll see this code:

```
var dataset = [];                        //Initialize empty array
for (var i = 0; i < 25; i++) {           //Loop 25 times
    var newNumber = Math.random() * 30;  //New random number (0-30)
    dataset.push(newNumber);             //Add new number to array
}
```

This code doesn’t use any D3 methods; it’s just JavaScript. Without going into too much detail, this code:

1. Creates an empty array called `dataset`.
2. Initiates a `for` loop, which is executed 25 times.
3. Each time, it generates a new random number with a value between zero and 30.
4. That new number is appended to the `dataset` array. (`push()` is an array method that appends a new value to the end of an array.)

Just for kicks, open up the JavaScript console and enter `console.log(dataset)`. You should see the full array of 25 randomized data values.

![Random values in console](https://alignedleft.com/content/03-tutorials/01-d3/90-the-power-of-data/7.png)

Notice that they are all decimal or floating point values (14.793717765714973), not whole numbers or integers (14) like we used initially. For this example, decimal values are fine, but if you ever need whole numbers, you can use JavaScript’s `Math.round()` method. For example, you could wrap the random number generator from this line

```
    var newNumber = Math.random() * 30;
```

as follows:

```
    var newNumber = Math.round(Math.random() * 30);
```

[Try it out here](https://alignedleft.com/tutorials/d3/the-power-of-data/5.html), and use the console to verify that the numbers have indeed been rounded to integers:

![Random integer values in console](https://alignedleft.com/content/03-tutorials/01-d3/90-the-power-of-data/8.png)

Next we’ll expand our visual possibilities with SVG.