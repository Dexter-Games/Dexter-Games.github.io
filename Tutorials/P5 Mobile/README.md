# P5.js Mobile Scaling Tutorial
We are going to draw a square in the middle of the screen. When clicked or tapped, it will change color. Most importantly, this square will be **approximately** the same size on every screen and always be positioned at the center of the screen. The idea is to make the sketch as mobile-friendly as possible.

## Getting Started
The first thing you need to do is **create a new replit project** using the **p5.js template**. You should see the following code:

```js
let colorlist = ['gold', 'yellow', 'turquoise', 'red']

function setup() {
  createCanvas(windowWidth, windowHeight);
    background(255);
}

function draw() {
  noStroke()
  fill(random(colorlist));
  ellipse(mouseX, mouseY, 25, 25);
}
```

We can remove most of this starter code. Delete the code contained inside the `draw` function. Now change the number in `background` to `0`. That will make the background of our sketch black. Your code should now look like this:

```js
let colorlist = ['gold', 'yellow', 'turquoise', 'red']

function setup() {
    createCanvas(windowWidth, windowHeight);
    background(0);
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

## Draw a Square
To draw a square in the center of the screen, we have to do some clever math rather than just using direct coordinates. We want the square in the center no matter what size the screen is. The center will always be `x=width/2, y=height/2` no matter what the size is. So that is where we will put the square.

```js
function draw() {
    square(width/2, height/2, 100)
}
```

But if you run the code, the square isn't **quite** in the center, is it? That is because the coordinate you give the square is actually the coordinate of its top-left corner. I would rather position the square based on the center. To do that, add a `rectMode` command to draw:

```js
function draw() {
    rectMode(CENTER)
    square(width/2, height/2, 100)
}
```

This is nice, but how do I change the size of the square based on the screen size? Well, we will need to create a variable which is how much the square should be scaled by. Let's call it `myScale`. `myScale` should be the size the screen is divided by the size we based it on. So let's also make a variable called `myBase` which is the size we base it on.

```js
var myBase = 500
var myScale = null

function setup() {
    createCanvas(windowWidth, windowHeight);
    myScale = height / myBase
    background(0);
}
```

**Note:** We set the value of `myScale` in `setup` because we can only get the screen `width` in **p5.js** functions like `setup`. (Also, remember, `null` is a value that just means **nothing**, we use it when we plan to add a value later)

Now we need to change the size of the square based on the scale. We will add a `squareSize` variable, give it a value during `setup`, and use that variable during `draw`.

```js
let colorlist = ['gold', 'yellow', 'turquoise', 'red']
var myBase = 500
var myScale = null
var squareSize = null

function setup() {
    createCanvas(windowWidth, windowHeight);
    myScale = height / myBase
    squareSize = 100 * myScale
    background(0);
}

function draw() {
    rectMode(CENTER)
    square(width/2, height/2, squareSize)
}
```

Try changing the size of the screen by dragging the edge of it. You'll notice the square size is based on how tall the screen is! But what if the width changes? We need an if statement to figure out which side of the screen is smaller, and scale based on that.

```js
function setup() {
    createCanvas(windowWidth, windowHeight);
    if(height < width) {
        myScale = height / myBase
    }
    else{
        myScale = width / myBase
    }
    squareSize = 100 * myScale
    background(0);
}
```

## Color

Let's add a variable called `myColor` and use it to color the square. Then, we will add some `mousePressed` code which will randomly pick a color from the colorlist each time you click or tap.

```js
let colorlist = ['gold', 'yellow', 'turquoise', 'red']
var myBase = 500
var myScale = null
var squareSize = null
var myColor = 'gold'

function setup() {
    createCanvas(windowWidth, windowHeight);
    if(height < width) {
        myScale = height / myBase
    }
    else{
        myScale = width / myBase
    }
    squareSize = 100 * myScale
    background(0);
}

function draw() {
    rectMode(CENTER)
    fill(myColor)
    square(width/2, height/2, squareSize)
}

function mousePressed() {
    myColor = random(colorlist)
}
```

## Final Experiment
Try to add some colors to the color list! What happens?