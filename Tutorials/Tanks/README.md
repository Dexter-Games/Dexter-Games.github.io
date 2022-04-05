# Tanks Tutorial
Let's create a tank game, which we will eventually make multiplayer, using **p5.js**, **p5.play**, and some pre-built tools for signaling to other players.

I will be using some free graphics found here: https://opengameart.org/content/tank-sprite
But feel free to use any tank images you'd like!

Let's start by copying/forking code from this replit project: https://replit.com/@jeremyglebe/P5-Game-Template
We're going to change everything in `script.js` - but the setup done in `index.html` and `style.css` will be useful!

## Getting Started
When you fork the replit project, your `script.js` will look something like this:

```js
var mycolors = ['gold', 'yellow', 'turquoise', 'red']
var othercolors = ['blue', 'green', 'purple', 'black']

function setup() {
    prConnect()
    createCanvas(windowWidth, windowHeight)
    background(200)
    setTimeout(pickRoom, 1000)
}

function draw() {
    if(mouseIsPressed){
        fill(random(mycolors))
        ellipse(mouseX, mouseY, 25, 25)
        prBroadcast({
        type: 'dot',
            x: mouseX,
            y: mouseY
        });
    }
    if(prHostID){
        fill('black')
        text("Room ID: " + prHostID, 10, height - 20)
    }
}

function prMessageReceived(peerID, data){
    if(data.type == 'dot'){
        fill(random(othercolors))
        ellipse(data.x, data.y, 25, 25)
    }
}

function pickRoom(){
    var id = prompt("Enter Room ID (enter nothing to create a new room)")
    if(id == ""){
        prCreateRoom()
        console.log(prHostID)
    }
    else{
        prJoinRoom(id)
    }
}
```

We are going to erase pretty much all of that. (It implements a multiplayer drawing game) Here is what we actually want to start with in `script.js`:

```js
function setup() {
    createCanvas(windowWidth, windowHeight)
}

function draw() {
    background(150, 255, 200)
}
```

(Also remember to upload your tank images to the replit project)

## Creating Sprites
A sprite is basically just an image with a ton of extra features and they usually correspond with an object in the game. To create a sprite, we need to start by loading images. (As we have done in previous tutorials) I am actually going to create one object to hold all of my images, then load images into specific variables inside that object. See the code below:

```js
var images = {
    tankBase: null,
    tankTurret: null,
    bullet: null
}

function preload(){
    images.tankBase = loadImage("tankBase.png")
    images.tankTurret = loadImage("tankTurret.png")
    images.bullet = loadImage("bullet.png")
}
```

Now I am going to create a tank object with two sprites: one for the base, and one for the gun. I will create the sprites in `setup` but the object needs to be created at the top, like usual. We will use the `createSprite` command, which we give an x and y coordinate to place the sprite at.

```js
// At the top of the file, put this code
var tank = {
    base: null,
    gun: null
}

// ...

// Somewhere further down, your setup function should now look like this
function setup() {
    createCanvas(windowWidth, windowHeight)
    tank.base = createSprite(200, 200)
    tank.gun = createSprite(200, 200)
}
```

Now add a command to the `draw` function to draw all sprites in the game.

```js
function draw() {
    background(150, 255, 200)
    drawSprites()
}
```

However, if you run this, right now our sprites are just squares! That is because we haven't given the sprites an image to be drawn with. Let's connect them with those tank images we loaded earlier using the `addImage` command.

```js
function setup() {
    createCanvas(windowWidth, windowHeight)
    tank.base = createSprite(200, 200)
    tank.gun = createSprite(200, 200)
    tank.base.addImage(images.tankBase)
    tank.gun.addImage(images.tankTurret)
}
```

Now you should be able to see your tank sprites on the game screen!

Quick note: If you need to adjust your tank's size, you can set it's `scale` variable. Values less than `1.0` make it smaller, and values bigger than `1.0` make it bigger. Example to make the tank twice as big:
```js
    tank.base.scale = 2.0
    tank.gun.scale = 2.0
```

## Moving the Tank
We probably want to be able to control this tank, right? Well, let's add some code to `draw` which will set the tank's velocity based on what keys we currently have pressed. We'll start with just moving up and down using the `W` and `S` keys.

```js
function draw() {
    background(150, 255, 200)

    // p5.play adds this command which can check, during draw, if a key
    // is pressed using it's string name
    // (p5.js has a similar command keyIsDown, but it only takes key codes)
    if(keyDown('w')){
        // Every sprite (like tank.base) has a variable called velocity which
        // represents its speed on the two axes, x and y. We can edit those
        // and the sprite will automatically move.
        tank.base.velocity.y = -2
    }
    else if(keyDown('s')){
        tank.base.velocity.y = 2
    }
    else {
        tank.base.velocity.y = 0
    }
    
    drawSprites()
}
```

Now we should make it so that the tank base is able to move left and right. You may have noticed that the gun is not moving with it, we'll fix that as well by updating it's `position` variable.

```js
function draw() {
    background(150, 255, 200)

    // Move up and down
    if(keyDown('w')){
        tank.base.velocity.y = -2
    }
    else if(keyDown('s')){
        tank.base.velocity.y = 2
    }
    else {
        tank.base.velocity.y = 0
    }

    // Move left and right
    if(keyDown('a')){
        tank.base.velocity.x = -2
    }
    else if(keyDown('d')){
        tank.base.velocity.x = 2
    }
    else {
        tank.base.velocity.x = 0
    }

    // Move the tank's gun to match it's position
    tank.gun.position = tank.base.position
    
    drawSprites()
}
```

Lastly, it would be nice for the tank to rotate in the direction it is moving. There are a few ways to handle this, but we are just going to directly set the `rotation` variable to an angle calculated with `atan2`. Below your movement code, add the following:

```js
    // Rotation handling
    // Set the angle mode of p5.js to match p5.play (sprites use DEGREES)
    angleMode(DEGREES)
    // Get the angle of the tank's movement
    // This is trigonometric math - if you aren't familiar with it don't worry
    // Basically atan2 calculates the angle based on the y and x distance of a line
    var angle = atan2(tank.base.velocity.y, tank.base.velocity.x)
    // Now set the tank's angle to the angle we calculated
    tank.base.rotation = angle
    tank.gun.rotation = angle
```

Now, this will work great **if** your tank's image happens to be facing to the right. That is because mathematically, zero degrees goes to the right. If your tank image happens to face any other direction by default, your angles are going to be all messed up here and you'll need to adjust them. My tank faces upwards, so I adjusted the angle 90 degrees. You should try different numbers if your tank faces a different direction.

```js
    // Rotation handling
    // Set the angle mode of p5.js to match p5.play (sprites use DEGREES)
    angleMode(DEGREES)
    // Get the angle of the tank's movement
    // This is trigonometric math - if you aren't familiar with it don't worry
    // Basically atan2 calculates the angle based on the y and x distance of a line
    var angle = atan2(tank.base.velocity.y, tank.base.velocity.x)
    angle = angle + 90 // Adjust for tank's default rotation
    // Now set the tank's angle to the angle we calculated
    tank.base.rotation = angle
    tank.gun.rotation = angle
```

One last fix, the tank always snaps back to the right position. That is because when it's velocity is zero, the `atan2` command also produces zero. I would like to make it so it only adjusts the angle if it's velocity x or velocity y are not zero. We're going to use an `if` statement that contains the symbol `||`, which means "OR". See our new draw code below:

```js
function draw() {
    background(150, 255, 200)

    // Move up and down
    if(keyDown('w')){
        tank.base.velocity.y = -2
    }
    else if(keyDown('s')){
        tank.base.velocity.y = 2
    }
    else {
        tank.base.velocity.y = 0
    }
    // Move left and right
    if(keyDown('a')){
        tank.base.velocity.x = -2
    }
    else if(keyDown('d')){
        tank.base.velocity.x = 2
    }
    else {
        tank.base.velocity.x = 0
    }
    // If tank.velocity.x does NOT equal zero OR tank.velocity.y does NOT equal zero
    if(tank.base.velocity.x != 0 || tank.base.velocity.y != 0) {
        // Rotation handling
        // Set the angle mode of p5.js to match p5.play (sprites use DEGREES)
        angleMode(DEGREES)
        // Get the angle of the tank's movement
        // This is trigonometric math - if you aren't familiar with it don't worry
        // Basically atan2 calculates the angle based on the y and x distance of a line
        var angle = atan2(tank.base.velocity.y, tank.base.velocity.x)
        angle = angle + 90 // Adjust for tank's default rotation
        // Now set the tank's angle to the angle we calculated
        tank.base.rotation = angle
        tank.gun.rotation = angle
    }
    // Move the tank's gun to match it's position
    tank.gun.position = tank.base.position
    
    drawSprites()
}
```

Note that `if(tank.velocity.x != 0 || tank.velocity.y != 0)` translates to: "*If the tank's velocity in the x direction is not 0 OR if the tank's velocity in the y direction is not zero, run the code below*"

## This tutorial is still a work-in-progress! There will be more later!