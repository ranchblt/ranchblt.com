+++
title = "Unity 2d Tutorial - Controlling character with your mouse"
date = "2017-05-31"
+++

**Unity version: 5.6**

This tutorial doesn't cover a lot of the reason why it's done this way or the basics.
This is a quick guide for specifically controlling your character with your mouse.

We'll be working entirely in a script on your main character. I called mine playercontroller.cs

## Camera
To start off you'll want to get the camera because we need to translate the mouse position to the world.
Add ``public Camera cam;`` to the controller.
It's good practice to allow it to be changed later but I'm lazy.
Let's make sure if it's not set to give it the default camera.
In your ``Start()`` function add

    if(cam == null)
    {
        cam = Camera.main;
    }

Great now we don't have to be bothered!

## Mouse controls character
If your controller doesn't have a function ``FixedUpdate()`` add it now.
This is different from ``Update()`` since ``FixedUpdate()`` runs once per physics timestep instead of once per frame.
Anything that gets the player input goes through ``Input``, you can get the mouse position with ``Input.mousePosition``
However that ``Vector3`` is a screen position, not a world position.
You can use the camera to convert it though.

    var rawPosition = cam.ScreenToWorldPoint(Input.mousePosition);

In this example I only want to move the character on the y axis, that is up and down.
I need to zero out the ``rawPosition``'s x.

    var targetPosition = new Vector2(0, rawPosition.y);

Serious note, I used a ``Vector2`` here because it makes more sense to me since the game is 2d.
However you need to lock the sprite in the Z axis because things get weird if you don't.
Otherwise you can use a ``Vector3`` and just set the Z axis to 0.

If you want to only allow X axis movement do this,

    var targetPosition = new Vector2(rawPosition.x, 0);

Now that we have the mouse position in the world vs the screen it's time to move our character.

    GetComponent<Rigidbody2D>().MovePosition(targetPosition);

*Test this now!*
The character should move on the axis you wanted but will be allowed to extend past the camera window.
That's not optimal.

## Lock character inside camera
Let's first lock the character to the camera.
We need to know the bounds of the camera but ``Screen`` only contains the screen size.
We'll use the same translation as before.
I created a class var for this ``private float maxHeight;``.

    var upperCorner = new Vector2(Screen.width, Screen.height);
    var rawUC = cam.ScreenToWorldPoint(upperCorner);
    maxHeight = rawUC.y

As I said before I'm locking this to a max Y.
If you wanted a max X just create a maxWidth var and set it to rawUC.x.

So far we have the max height, now what?
There is something to help us in the ``Mathf`` lib.
Just use it and trust me.
[Here's the documentation if you want.](https://docs.unity3d.com/ScriptReference/Mathf.Clamp.html)

    float targetHeight = Mathf.Clamp(targetPosition.y, -maxHeight, maxHeight);
    targetPosition = new Vector2(0, targetHeight);

*Test this now!*
We're locking in ``targetPosition`` inside our world.
You should not be able to get your character outside the camera.
Oh damn though, you noticed half of the character is outside the camera.
Fine let's continue.

## Fix character sprite going off screen
We need to get the size of the character.
Again I'm doing this for height you can figure out for width.
[Here's the documentation if you want.](https://docs.unity3d.com/ScriptReference/Bounds-extents.html)

    var charHeight = GetComponent<Renderer>().bounds.extents.y;

Extents is half the size, lucky us.
Make sure to subtract this from maxHeight,

    maxHeight = rawUC.y - charHeight;

*Test this now!*
Actually we should be done.

If something is wrong or you need more help.

[twitter](https://twitter.com/peppage)

[github](https://github.com/ranchblt/ranchblt.com)
