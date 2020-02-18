A D3.js v4 Example/Tutorial
In D3.js examples a common theme that pops up is the use of a fixed size svg. I typically have to make the same set of modifications to get a resposive svg.

Here is a simple example (I hope) of said modifications. I took this original bar chart from mbostock based on D3.js v4 and split up the code to make the chart responsive when:

Data is loaded
The browser window / svg is resized
Open in a new window and resize to see it in action.

The original logic has been split up into SETUP, DRAWING, and LOADING DATA.

For example:

Setting the axis range is dependent on browser/svg size, so it goes in DRAWING.
Setting the axis domain is dependent on the data itself, so it goes in LOADING DATA.
SETUP contains stuff that does not change (is neither size nor data dependent).

https://bl.ocks.org/alanvillalobos/14e9f0d80ea6b0d8083ba95a9d571d13


#Great File
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
