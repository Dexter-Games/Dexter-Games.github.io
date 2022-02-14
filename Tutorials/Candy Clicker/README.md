# Candy Clicker Tutorial
We are going to make a clicker game, which is a simple game where you click on an object to increase some score, money, or other number. One example of an early clicker game is [Cookie Clicker.](https://orteil.dashnet.org/cookieclicker/) Our clicker game is going to be much simpler. We will show a piece of candy and, when clicked, raise a score value. Of course, you can always add more features as you learn more!

![Candy Clicker](candy_clicker.gif)

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

We can remove most of this starter code. Delete the `colorlist` variable as well as all the code contained inside the `draw` function. Your code should now look like this:

```js
function setup()
{
  createCanvas(windowWidth, windowHeight);
  background(255);
}

function draw()
{
  
}
```

## Using an Image
Next we need to load an image into the game to be our candy. You can use the following image, if you'd like. You should be able to right click it to save it, then upload it to your replit project.

![Piece of Candy](candy.png)