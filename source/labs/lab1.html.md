;;;
"title":"Planetoids",
"lesson_number": 3
;;;

Part 1: Planetoids!
===================

This lab will guide you through coding up a web-based implementation of a game we call planetoids (that may or may not rhyme with the name of another space-related video game from the 1980s). You will need to bring your laptop to the lab or use one of the mac computers in Morril 285. Unfortunately, tablets will probably not work very well unless you have an external keyboard.

This example will use a programming language that is practically indistinguishable from C and C++ programming. (Note: If you are familiar with C or C++ the primary difference you will see is that there is no main() function and instead the draw() function serves this role.)

In Planetoids, there is a little ship that flies around in space, very far away from stars and planets so that the gravitational acceleration is zero.

The code is designed to solve the (rather simple) kinematic equations in this case. If we just consider the x-direction forces the equations look like this:

$$ \sum F_{\rm{net},x} = F_{\rm thrust} = m \, a_x $$

$$ \Delta v_x = a_x \Delta t $$

$$ v_x = v_{x0} + \Delta v_x $$

$$ x = x_0 + v_x \cdot \Delta t$$

The computer program we will work with here computes these equations over and over again, updating vx and x depending on whether the thrust is turned on or off. Since the thrust is the only force, if it is turned off then ax=0 and Δvx=0 and the ship just continues with the same vx velocity. This is how the code works if the ship only moves in the x direction. The equations are essentially the same for the y direction. In the last step of this lab you will modify the code so that the ship can move in the y direction too.

## Step 1. Play around with a 1-dimensional version of Planetoids

Click on this link to the 1D version of the spaceship from planetoids. Push up arrow to see the rocket accelerate. Unfortunately right now the ship can only accelerate in one direction. We will change this step-by-step to make the behavior more similar to the classic planetoids game. If the example doesn't work tell the instructor.
This is the code used to move the ship around in the 1D version. Take a close look at the code and try to get an idea of what each part does:

    void draw() {
      
      // Update velocities
      vx += deltaVx;

      // Update location
      x += vx*dt;

      // Set deltaV to zero (thrust off unless user turns it on)
      deltaVx = 0;

      // Turn or thrust the ship depending on what key is pressed
      if (keyPressed) {
        if (key == CODED && keyCode == LEFT) {
          theta += 0.0;
        } else if (key == CODED && keyCode == RIGHT) {
          theta += 0.0;
        } else if (key == CODED &&  keyCode == UP ) {
          // Rockets on!
          float accelx = Fthrust*cos(theta)/mass;
          deltaVx = accelx*dt;
        } else if (key == CODED &&  keyCode == DOWN ) {
          // Do nothing
        } 
      } 

      // Draw ship and other stuff
      display();

    } // end draw()

## Step 2. Download the 1D planetoids code and get it running on your own machine.

Create a folder (on the Desktop or elsewhere) called planetoids

Be very careful to not change the file extensions (for example click Don't Append in Mac) and download two files to the planetoids folder. You will need to download functions.pde, and planetoids.pde. Right click on the links and click Save (or on a Mac hold control and click on the file) to download these files. You may be asked to change the file extensions but the right answer is always no or "Don't Append". The program won't work if you change the extensions of these files.

Inside of the planetoids folder, double click on the planetoids file and this should open up the Processing Development Environment. When you press  you should get the same behavior as in step 1.

Step 3. Edit the source code to let the ship rotate!

Now the fun begins! Change these lines:

    if (key == CODED && keyCode == LEFT) {
      theta += 0.0;
    } else if (key == CODED && keyCode == RIGHT) {
      theta += 0.0;

to look like this:

    if (key == CODED && keyCode == LEFT) {
      theta += 0.05;
    } else if (key == CODED && keyCode == RIGHT) {
      theta += -0.05;

Remember to save your changes! Now press  and see if you can rotate the ship using the left and right arrows. If you feel like making theship rotate faster or slower in response to the key being pressed then change the 0.05 to something larger or smaller.

The planetoids game is now set up so so that only the x-component of the thrust is what accelerates the ship. Notice that allowing the ship to rotate means that we can speed up and slow down the ship because we can rotate the rocket and apply a force that is opposite to the velocity vector. Our planetoids game is already a lot more fun! For reference, the game should now behave like this.

## Step 4. Enable multi-dimensional space travel (i.e. let the ship move in the y direction too)

Now take the planetoids.pde code you've written and modify it so that the ship can move in the y direction. To do this look closely at the lines that update the x positions and the x velocities, etc., and make a copy of these lines to update the y position. For example: Change this:

    x += vx*dt;

To this:

    x += vx*dt;
    y += vy*dt;

Make sure to update the y position (y), the y velocity (vy), the change in the y velocity (deltaVy) and the y acceleration (accely).

If you make all these changes your code should behave like this and your ship should be able to move in 2 dimensions like the original video game.

Potential pitfall: Did you use cosine or sine to determine the y-component acceleration?

Potential pitfall: Remember that the +y direction is down, but a +45 degree angle points to the top right. Are you missing a minus sign somewhere?

Check in with the instructor when you get to this stage!

Challenges: Choose two or more

### Option #1: Add Reverse thrusters

This is one of the easier challenges. Notice that planetoids.pde has a section for when you press the down arrow:

    } else if (key == CODED &&  keyCode == DOWN ) {
      // Do nothing
    } 

Where it says "Do nothing" how would you add code to fire the thrusters in reverse?

Hint: You want to accelerate in the opposite direction of the acceleration you get when the thrusters are turned on.

### Option #2: Add planetoids to the game

It's not much of an planetoids game without planetoids! Add planetoids to the code which drift at random directions using the ellipse function

    ellipseMode(RADIUS);
    fill(255); // Inside of ellipse is set to be white
    ellipse(x_planetoid,y_planetoid,width_planetoid,height_planetoid);

Place this just after where it says display(); Then make sure these variables are initialized at the beginning of the program like this:

    float x_planetoid=200;
    float y_planetoid=200;
    float width_planetoid=50;
    float height_planetoid=50;

It is up to you to make the planetoid move in a random direction with some constant velocity.

Note: to calculate a random direction use the random function to generate a fraction between 0 and 1. For example:

float random_number = random(1.0);

### Option #3: Add a projectile to the game

This option is the hardest. You can add another "else if" to check to see if you've pressed the spacebar:

    } else if (key == ' ') { // spacebar is pressed
      // Do nothing
    } 

How would you modify the code so that the spacebar shoots a projectile from the nose of the ship? Add two new float variables called xprojectile and yprojectile to the beginning of the program and use the "point" function to show the position of the projectile. Place the following code just after display(); and not before!

    point(xprojectile,yprojectile);

Placing the above code just after display(); This makes sure that it is drawn every time. Set xprojectile and yprojectile to zero initially so that the point is drawn at the top left corner of the grid and you can't see it. When the spacebar is pressed set the xprojectile and yprojectile equal to the position of the ship. That at least will put a dot on the screen where the ship is. How would you make it move?

### How to share your program!

If you want to share your program with a friend, send them three files: Your planetoids.pde, a copy of functions.pde and right-click and download the file planetoids.html. As long as these three files are in the same folder on your or someone else's computer you should be able to double click on planetoids.html and play the game on a web browser. You don't need to install the processing interactive environment to get this to work.

If this seems overly complicated the other way you can share your program is by clicking "Create sketch" at openprocessing.org. Upload your code, set up an account (for free!) and then give your friends a link to the sketch.