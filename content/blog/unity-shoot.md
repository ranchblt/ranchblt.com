+++
title = "Unity 2d Tutorial - Shooting an animated projectile"
date = "2017-06-05"
+++

**Unity version: 5.6**

This tutorial doesn't cover a lot of the reason why it's done this way or the basics.
This is a quick guide for shooting something from your main character without gravity when a button is pressed.

# Create animation from sprite sheet

When you import a sprite sheet you must set the sprite mode to Multiple in the Inspector.
Open up the sprite editor.
This is the screen you can define the shape of your sprite.
There are options to auto create the sprites based on size and unity taking a guess.
Here I just did it myself because there are only 3 frames.
Just draw a box around each one, order matters.

<img src="http://i.imgur.com/lux2rq9.jpg">

Now when you drag the fireball onto your Scene it will suggest you create an animation.
Do this, name it something obvious.
Since we want to shoot it out of our character we should give it a Rigidbody 2d.
For simplicity set the Gravity Scale to 0.

Ok this is something interesting I picked up.
You want this fireball to be useable on other game objects but you don't want it to be in the scene on the start.
Drag the fireball from the Scene to the Project (I suggest in a prefabs folder) and it saves the fireball in its current state.
This is the object you drag to the character, you can always drag this prefab back to the Scene for testing.
Now you can remove the fireball object from your scene.

# Add projectile to character

Start by adding a public var for the fireball so we can add it in Unity.
Also let's make the speed adjustable.

    public Rigidbody2D fireball;
    public float fireballSpeed = 8f;

Then in ``Update()`` check if the Input button that we want to use is down and create a new instance of the fireball.
Changing the velocity of the new fireball makes it move to the right on the screen.

    if (Input.GetButtonDown("Fire1"))
    {
        var fireballInst = Instantiate(fireball, transform.position, Quaternion.Euler(new Vector2(0, 0)));
        fireballInst.velocity = new Vector2(fireballSpeed, 0);
    }

Oh the Euler?
No idea.
This code works and I'm going with it.

Drag and drop fireball prefab to the wizard script.
*Test this now!*
You'll see in the Hierarchy that new fireballs are created every time you press Mouse1 or left ctrl.

# Bonus: Destroy offscreen projectiles

If you're shooting a lot you'll want anything off screen to be destroyed.
Or else you're going to waste a lot of physics calculations.
Always good practice to clean up after yourself.

Create an empty object in your scene.
Rename it, I picked "Destroyer".
add an Edge Collider 2d to the object.
We want to set it as a trigger so click "Is Trigger".

Now you want to Edit Collider and place it around the outside of your game.
It's pretty intuative you can add points by clicking and remove points by ctrl clicking.
I put it mostly around the camera so that it would collect other objects besides these fireballs.
It doesn't have to be perfect.

<img src="http://i.imgur.com/NlezsH3.jpg" />

Now create a new script on that object, I named mine "DestroyOnContact".
Since it's a trigger and it's 2d use this function.
The collision is passed in so we just need to destroy the gameObject that it's attached to.

    private void OnTriggerEnter2D(Collider2D collision)
    {
        Destroy(collision.gameObject);
    }

*Test this now!*
This happens off camera so you'll either have to just see in the Hierarchy that the fireballs are deleted or switch back to the Scene.

# Bonus 2: Collision

Really all you have to do is throw a Box Collider 2d on the fireball prefab.
I ran into something weird where when my monster and fireball both had a Box Collider 2d.
When the objects ran into each other the fireball pushed the monster off the screen.
I tried to use ``OnCollisionEnter2D(Collision2D collision)`` but it only worked sometimes.
I fixed this by switching the Box Collider 2d on the monster to Is Trigger and using ``OnTriggerEnter2D(Collider2D collision)``.

If something is wrong or you need more help.

[twitter](https://twitter.com/peppage)

[github](https://github.com/ranchblt/ranchblt.com)