# Snake Starter Tutorial
We are going to make a sort of copy of the classic snake game. Not sure what snake is? Here is what it generally looks like. (Though many different versions exist that look slightly different)

![Snake Game](snake.gif)

## Getting Started
To help you get started, we are going to write code to get a square moving like the head of the snake! The first thing you need to do is **create a new replit project** using the **p5.js template**. You should see the following code:

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

Next, I would like to create an object variable that represents the snake in the game. Recall that an **object** is a **variable which contains multiple other variables**. We use these to represent characters or things in our game which need multiple variables to describe them. For instance, we might want all of the following information about the snake: location, size, direction, and color. It would be best if we could package all this information into one object. We could create this object using the following code:

```js
// Snake object variable
var snake = 
{
  color: 'green', // Maybe we could customize the color for each player later?
  size: 15, // This is the size of one square of the snake
  direction: 'none', // 'none', 'up', 'down', 'left', or 'right'
  x: 0, // This is the X coordinate of snake's location
  y: 0 // This is the Y coordinate of snake's location
}
```

I will create that object at the very top of my code, above the `setup` function.

## Controlling With The Keyboard
Next we will want to create code that controls the `direction` of the snake using keyboard keys. To do so, we have to add a new special **p5.js function** to the bottom of our code. The function is called `keyPressed`. Here is what our code should currently look like:

```js
// Snake object variable
var snake = 
{
  color: 'green', // Maybe we could customize the color for each player later?
  size: 15, // This is the size of one square of the snake
  direction: 'none', // 'none', 'up', 'down', 'left', or 'right'
  x: 0, // This is the X coordinate of snake's location
  y: 0 // This is the Y coordinate of snake's location
}

function setup()
{
  createCanvas(windowWidth, windowHeight);
  background(255);
}

function draw()
{
  
}

function keyPressed()
{
  
}
```

Notice the new function `keyPressed` at the bottom? Code inside that function will run **every time any key is pressed**. We will want to check what key is being pressed to decide what to do. This is where we can use the `if`, `else if`, and `else` statements to make decisions in code. Recall that **if statements only run when their condition is true**. The **condition** is the variable, comparison, or other code you put inside the parentheses of the if statement. Look at the following code:

```js
function keyPressed()
{
  if (keyCode == UP_ARROW)
  {
    // This runs when the up arrow key is pressed
  }
  else if (keyCode == DOWN_ARROW)
  {
    // This runs when the down arrow key is pressed
  }
  else if (keyCode == LEFT_ARROW)
  {
    // This runs when the left arrow key is pressed
  }
  else if (keyCode == RIGHT_ARROW)
  {
    // This runs when the right arrow key is pressed
  }
}
```

`keyCode`, as well as all the `[DIRECTION]_ARROW` variables, are special variables that are created by **p5.js**. `keyCode` is a variable which tells us what key was pressed to make this code run. (There is another variable which works like `keyCode` called `key`. The difference is that `keyCode` stores special keys, like arrows and spacebar, while `key` stores letter and number keys, like `w` or `1`.) The arrow variables are just there to represent keys you can compare `keyCode` to.

Now we want to change the `direction` variable in the `snake` object when those buttons are pressed. Recall that we use the dot (`.`) to access variables inside objects. Look at the code below.

```js
function keyPressed()
{
  if (keyCode == UP_ARROW)
  {
    // This runs when the up arrow key is pressed
    snake.direction = 'up'
  }
  else if (keyCode == DOWN_ARROW)
  {
    // This runs when the down arrow key is pressed
    snake.direction = 'down'
  }
  else if (keyCode == LEFT_ARROW)
  {
    // This runs when the left arrow key is pressed
    snake.direction = 'left'
  }
  else if (keyCode == RIGHT_ARROW)
  {
    // This runs when the right arrow key is pressed
    snake.direction = 'right'
  }
}
```

## Drawing the Snake
So, we have keyboard controls that update the `direction`. But right now, that direction doesn't actually **do** anything. So we need to write code to move and draw the snake, and we need to base that movement on the direction. Recall that the **draw** function will run **60 times per second**. That is because our **frame rate** is **60 frames per second**. **Frame rate** is a term which means **how often the computer re-draws the screen**. We can change the frame rate with a special **p5.js** function, and we probably should. For the classic game of snake, the frame rate is what determines the difficulty. Let's change that in the **setup** function.

```js
function setup()
{
  frameRate(5) // This changes the frame rate to 5 frames per second
  createCanvas(windowWidth, windowHeight);
  background(255);
}
```

Now that we are using a lower frame rate, let's change the draw function to make the snake head appear as a square. We will use the **square** and **fill** commands to do that.

```js
function draw()
{
  // Draw the snake
  fill(snake.color) // Set the color of the snake
  // Draw the square at the snake's x and y with a side length equal to the snake's size
  square(snake.x, snake.y, snake.size) 
}
```

Now the square needs to move! So we will need more **if statements** which will check the snake's direction and then update its **x** and **y** based on that direction. We will make the snake move a number of pixels equal to its size each time we draw. In the classic snake game, the snake usually moves a full square each time it moves, so we are copying that idea. Take some time to really read and think about the following code:

```js
function draw()
{
  // Move the snake
  if(snake.direction == 'up')
  {
    snake.y = snake.y - snake.size
  }
  else if (snake.direction == 'down')
  {
    snake.y = snake.y + snake.size
  }
  else if(snake.direction == 'left')
  {
    snake.x = snake.x - snake.size
  }
  else if (snake.direction == 'right')
  {
    snake.x = snake.x + snake.size
  }
  // Draw the snake
  fill(snake.color) // Set the color of the snake
  // Draw the square at the snake's x and y with a side length equal to the snake's size
  square(snake.x, snake.y, snake.size) 
}
```

Remember that since draw runs every frame, and we have set the frame rate to 5, the snake will move 5 times each second. Now, when you run your code you should notice something **really weird**. Every time we draw a new snake head, the old one is still there! This does kind of make it look like the old snake game, but we only want it to show the snake's body when the snake gets bigger. Right now, it should be erasing those old head squares. Well, the issue is that we aren't erasing the old frame each time we draw a new frame. So to fix this, let's paint over the old frame at the start of each `draw`. We can paint over everything currently on the screen with the `background` command. (Which you might notice is already run once during `setup`) We just add one line, look at the following code:

```js
function draw()
{
  // Paint over the old frame
  background(255) // 255 means a white background, more on this in a later lesson
  // Move the snake
  if(snake.direction == 'up')
  {
    snake.y = snake.y - snake.size
  }
  else if (snake.direction == 'down')
  {
    snake.y = snake.y + snake.size
  }
  else if(snake.direction == 'left')
  {
    snake.x = snake.x - snake.size
  }
  else if (snake.direction == 'right')
  {
    snake.x = snake.x + snake.size
  }
  // Draw the snake
  fill(snake.color) // Set the color of the snake
  // Draw the square at the snake's x and y with a side length equal to the snake's size
  square(snake.x, snake.y, snake.size) 
}
```

## Conclusion
That's it! You have the basic movement of the snake game down! At this point we will want to make food for the snake appear, and increase the score when we eat that food. (We aren't going to make the snake get bigger in this first project, just increase the score) We'll talk about how to do that in the next lesson. For now, congratualations on getting this far!

## Bonus Feature: Removing Scrollbars
Some of you might be experiencing a weird bug with the way our game appears in the browser window. There might be scroll bars on the side and bottom of your game, which keep moving when you use the arrow keys to control the snake. That is because our **HTML document**, which our **p5.js canvas** sits inside of, has something called **padding**. **Padding** means that everything **inside** an element is pushed **inward a few pixels**. Another similar concept, **margins**, means that elements **outside** of the given element are pushed **away a few pixels**. Lastly, there is a concept called **overflow** which refers to how a document handles content outside of the screen. To fix our issue with the scrollbars, we want to remove any **padding** and **margins**, as well as changing the **overflow** mode to **hidden**. (Which means don't show anything outside the screen) We can fix all of this using **CSS Styles**. Go to the `style.css` file in your **replit** project. It should be completely empty. Add the following code:

```css
html, body
{
  padding: 0;
  margin: 0;
  overflow: hidden;
}
```

Now run your game. It should look quite nice!