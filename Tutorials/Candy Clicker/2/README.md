Now change the number in `background` to `200`. That will make the background of our sketch gray. Your code should now look like this:

```js
let colorlist = ['gold', 'yellow', 'turquoise', 'red']

function setup() {
    createCanvas(windowWidth, windowHeight);
    background(200);
}

function draw() {

}
```

Run the code. Notice the annoying white bars on the sides of our sketch? And the scrollbars? Lets remove those. To do so, we actually have to edit the `style.css` file instead of the `script.js` file we have been editing.

The white outline is because our **HTML document**, which our **p5.js canvas** sits inside of, has something called **padding**. **Padding** means that everything **inside** an element is pushed **inward a few pixels**. Another similar concept, **margins**, means that elements **outside** of the given element are pushed **away a few pixels**. Lastly, there is a concept called **overflow** which is how a document handles content outside of the screen. To fix our issue, we want to remove any **padding** and **margins**, as well as changing the **overflow** mode to **hidden**. (Which means don't show anything outside the screen) Add the following code to `style.css`:

```css
html, body
{
  padding: 0;
  margin: 0;
  overflow: hidden;
}
```

## Adding the P5.Play library
```html
<script src="https://cdn.jsdelivr.net/gh/jeremyglebe/p5.play@master/lib/p5.play.js"></script>
```