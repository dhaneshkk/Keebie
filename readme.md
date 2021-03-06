# Keebie

more keyboards = faster hacking



#### What does it do:

Keebie is basically a small script for assigning and executing commands on a second (or third, fourth, etc., the only limit should be how many ports you have) keyboard. So you can make the spacebar search for a window and paste something. or make the M button run a script.



#### How to set up:

Okay so this point is still a little bit uh, well. experimental. I am not very experienced with permissions systems in linux and this involves that so if you know a better way of doing what I'm doing , please tell me. Anyways, with that out of the way, lets go over what you need to do to set this up

- **Find the input file of your target keyboard**
  - You can do this by running `ls /dev/input/by-id/`, and looking for a name that looks like it matches. Test to see if you've found it by running `cat /dev/input/by-id/[the-device-you-think-it-is]`( this will most likely have event and/or kbd in the name ) There will likely be more than one. keep trying that command and pressing buttons on your target keyboard. once your console starts to output garbage when you press buttons on the target keyboard, you've found it. note that down
- **Give yourself permission to access that device.**
  - This is that part I wasnt sure about. I suspect that the way I did it was wrong, but I'll give you the way I did it, and the way that seems like the right way but didnt work for me.
  - What I ended up doing was `sudo chmod a+r /dev/input/by-id/[my-device-name]`
  - What is typically done: most inputs should belong to a group ( usually `input`, but check by using `ls -l /dev/input/by-id/[my-device-name]` ), and you should be able to add yourself to whatever group the device belongs to by doing something like `usermod -a -G input yourusername`. In my case the device didnt seem to belong to any group except for root.
- **Add your device file directory to the first line of the config file** (no trailing spaces, please.)

And that should have you up and ready to roll. If when you run the script, you get an error saying something about permission denied, something has gone wrong with step 2. Good luck lmao.



#### Usage:

Just dump the files anywhere and run `python keebie.py` and if everything it up and running, you should be good to go. Test your second keyboard by pressing spacebar which is bound to a test message by default. To run with more keyboards, run the script again with the `--device <dev-id>` option for however many devices you want to add. 

#### Options:

`--device <device-id>` Launches th script attatched to specified device, creating a new layer file specifically for it if it doesnt already exist. Press ESC to return to default layer.

 `--add`: Launches into the script addition shell. Instructions should be pretty straightforward. Any commands entered will be launched, and if you try to bind the same key twice, your new value will overwrite the old one. there is some special syntax that can be used in this entry that will allow for special functions:

- `layer:<layername>`: Will create a layer file in  `/layers/`, and let you bind switching to it to any key
  - ( this will create a layer with a default layout of only having `ESC` return you to the default layer, you can add to it by launching the script, switching to it, then running `python keebie.py -a` again )
- `script:<scriptname.sh (options)>`: Will launch a `< scriptname.sh (options) >` from `/scripts/`
  - ( same as `bash ~/whereveryourfolderis/scripts/scriptname.sh (options)` )
- `py:<pythonscript.py (options)>`: Will launch `< pythonscript.py (options) >` from `/scripts/` 
  - ( same as `python ~/whereveryourfolderis/scripts/pythonscript.py (options)` )

`--list`: Lists all layer files and all of their contents

`-h` or `--help`: Shows a short help message



#### Some tips:

**Multi-script drifting**

​	put an `&` at the end of your commands. this will effectively make any commands you run through it into their own process and keep from any long winded scripts or error messages keeping the rest of your macros from responding. I considered putting this feature in by default, but decided against it since I assume the only people who will be using it would prefer to have the choice left up to them, and I would prefer not to write a system for swapping it in and out.