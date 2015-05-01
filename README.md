
**Rocket Defence** was built to show how easily you can create a game in
**Marmalade Quick**, an open source, Lua-based rapid 2D development engine
that comes bundled with the Marmalade SDK.

https://youtu.be/5xSIVPykQuw

Quick uses **Lua**, an easy to use high level scripting language, but
underneath it binds tightly to C++ using Marmalade and Cocos2d-x APIs for
features and performance.

This readme guides you through importing, running, improving and pushing
the game to a device. It should take about 5 minutes to complete but why not
take a bit more time to digest and appreciate the code :)

The project includes two versions of the main game code:

- main.lua is the "start" version, missing the effects added by tasks 2 and 3
  below
- mainCompete.lua has those effects added

Edit resorces/app.icf and switch between versions by commenting/uncommenting
the relevant lines.

Rocket Defence was based on https://github.com/nickchops/QuickGameIn100Lines
with some re-skinning and additional features

It uses an asteroid texture from http://opengameart.org/users/phaelax and
missile texture from http://opengameart.org/users/napoleon - 
both are under CC licenses: http://creativecommons.org/licenses/by-sa/3.0/

Built with Marmalade 7.7.0. If using another version, you need to follow steps
here:  http://docs.madewithmarmalade.com/x/oB2m
You need 7.7.0 or newer to use the filter effects in task 2.

#Tasks:

## Import the project into Marmalade and run on desktop

Marmalade uses .mkb files for its projects. We'll import the game into the Hub
(marmalade's core project manager tool) and run it in the desktop Simulator...

- Copy the RocketDefense folder to the Desktop
- Open Marmalade's Hub (the blue/white M logo on the Windows task bar)
- Click "Marmalade Quick (Lua)" > Import Project > Desktop RocketDefense.mkb
- Click Open Project and the bottom to go to the project build view
- Set the "Platform" drop down (middle left) to "Simulator"
- Click the big green run button

To play, just touch to fire rockets to stop the endless meteors!
The Simulator supports full multitouch if you've got a touch screen PC.


## Check out the code in the IDE

- Close the Simulator if running and click the "OPEN IN IDE" button in the Hub
  ...The project opens in the ZeroBrane Lua IDE.
- Open the "resources" folder on the left. This contains all code and assets.
- Double click main.lua. This contains all the game code!
  Note that it has only 151 lines of user code, including comments.
  The project also uses some typical open source helper libs:
  - NodeUtility: helper for working with scene graph nodes
  - VirtualResolution: flexible helper for scaling to any screen size.

  
## Let's add some effects!

Lets add a shader-based "screen burn" effect to the sky when the player
fails to stop a meteor.

1) Replace the line with  --> TASK 1 <-- with the following:

        sky.filter.name = "brightness"

2) Replace the line with  --> TASK 2 <-- (line 134, towards the end of the file)
   with the following, which mutates the brightness on missing a meteor:
   
        sky.filter.intensity = 1
   
3) Replace --> TASK 3 <-- right near the end of the file with the following,
   which resets the effect:

        if sky.filter.intensity > 0 then
            sky.filter.intensity = sky.filter.intensity - 0.01
        end
            
Press F5 or click the green triangular run button to test in Simulator. 


## Lets add some particle effects!

1) Stop the Simulator, then replace --> TASK 4 <-- with the following to create
   fire particles trailing from the rocket:
   
        rocket.tail = director:createParticles("Comet.plist")
        rocket.tail.sourcePos = { rocket.x, rocket.y }
        
2) Replace --> TASK 5 <-- with the following so particles follow the rocket:

        if obj.tail then
            obj.tail.sourcePos.x=obj.x
            obj.tail.sourcePos.y=obj.y
        end

3) Replace --> TASK 6 <-- with the following to shrink particles on collision:

        if obj.tail then
            obj.tail.startSize = obj.tail.startSize/3
            obj.tail.endSize = obj.tail.endSize/3
        end

4) Finally, replace --> TASK 7 <-- with this, to stop the particles:

        if obj.tail then
            obj.tail:stop()
        end

Press F5 or click the green triangular run button to test again.


## Put it on a phone!

- Switch back to the Hub
- From the "Platform" dropdown, pick a phone platform
  (e.g. "Windows Phone 8.1")
- From the "Architecture" dropdown, pick ARM (e.g. "wp81-arm")
- Under BUILD set both switches to "Release" (click them if needed)
- Make sure the phone is attached and the screen on the phone is unlocked.
- Click the big green "Package, Install (and Run)" button at the top!
- The game should install/run! On Windows Phone, If the phone doesn't start
  it automatically, swipe left on the home screen and scroll down to "R" to
  find it.
- Rotate the phone to check out the virtual resolution in action.
