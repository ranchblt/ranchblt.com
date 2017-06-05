+++
title = "Unity 2d Tutorial - Simple character animation"
date = "2017-06-01"
+++

**Unity version: 5.6**

This tutorial doesn't cover a lot of the reason why it's done this way or the basics.
This is a quick guide for giving your character a different sprite when a button is pressed.

For this tutorial I'll be using 2 different sprites to switch between when my wizard shoots.
We'll set it up so the wizard switches on "fire1".

# Create animation
First open up the animation window.
From the menu, Window -> animation.
Dock that panel, I'll wait.
Now select your your character, I assume they're in the scene already, and the animation window should change and show a create button.

<img src="http://i.imgur.com/u7zRYxJ.jpg" />

Click it, give your new animation a name related to your character and the action.
I suggest idle or standing or you get it.
It will show up in a dropdown so it is important to name correctly.
Now add a new propery and in the menu, expand Sprite Renderer and scroll all the way to the bottom.
Click the plus next to sprite, you may need to scroll the menu to the right.

<img src="http://i.imgur.com/7VBy4L0.gif" />

Since I'm not adding a walking animation you can simplify this animation by deleting the second key frame.
Here:

<img src="http://i.imgur.com/3jER97i.jpg" />

Now we need to add the animation of the wizard shooting.
In the area you added a property there is a dropdown with what you named the animation.
Click it and then click "Create New Clip...".
Again give it a decent name, you'll thank yourself later.
This creates an empty animation, it's just easier if you copy the first animation to this one.
Go back to the first animation, select the keyframe and copy and paste it to the new animation.
Now we just need to switch it to your other shooting sprite.
Drag the sprite you want to use to the Sprite Renderer in the Inspector.

<img src="http://i.imgur.com/ermBSnX.gif" />

If you have the Scene open you'll be able to see the change.

At this point you might be wondering why the play buttons at the top are red.
It has to do with the record button in the Animation window.
We're just going to ignore that for now, turn off the record button.

# Setup triggers
Now we have to setup a trigger so you can switch the animations in code.
Switch to the Animator panel and you'll see a nice state machine.
Organize the buttons to make it a little clearer if you want.

On the left of the Animator panel if you have parameters selected there is a little plus.
Click it and select trigger.
Make sure to name your trigger carefully because this is the name we use in code.

<img src="http://i.imgur.com/QSQcKlB.jpg" />

Now right click and select Make Transition on the first animation.
Then left click on the second animation.
Note: In my image the first animation is the orange one, it already has a transition from Entry.

Select the transition we just created and in the Inspector you'll see Has Exit Time.
Uncheck this box.
This stops the transition from transition at the end of the animation.
For example, after my test animation ends in 1 second in would transition to temp-shoot.
We don't want that, we want to use the trigger.

So uncheck that box and use the + under Conditions to add the shoot trigger as a condition.

<img src="http://i.imgur.com/FZ2IUNH.jpg" />

Create a new trigger on the other animation and point it back to the first one.
The same process as before by right clicking on the animation.
Since we want this one to auto transition back to the first animation we don't need to uncheck Has Exit Time.

*Test this now!* You'll need to be able to see both the Game and Animator windows but you can test the transition.
Run the game and in the Animtaor window click the radio button next to the shoot trigger and it will activate the trigger.
You should see the transition happen in the Game.

# Code

This part is pretty easy.
First let's add a new private var to hold the Animator.
This doesn't change after load so it can be added to start.
This is a snippet:

    private Animator anim;

    void Start() {
        anim = GetComponent<Animator>();
    }

Then just use Input to detect the button change and change the animation.

    void Update () {
        if (Input.GetButtonDown("Fire1"))
        {
            anim.SetTrigger("shoot");
        }
    }

*Test this now!*
It should switch the animation when you press Fire1.
The default buttons for this are left ctrl and mouse 1.
You can change it though.
From the main menu, Edit -> Project Settings -> Input.
This opens the InputManager in the Inspector.

If something is wrong or you need more help.

[twitter](https://twitter.com/peppage)

[github](https://github.com/ranchblt/ranchblt.com)
