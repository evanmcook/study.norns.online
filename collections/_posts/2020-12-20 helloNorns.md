---
layout: post
title: "hello norns"
author: "@evancook.audio"
date: 2020-12-27
---
for many years, i've seen and wondered about monome hardware, but either could not understand *what exactly was going on over there* because of my own progress in understanding audio, or could not afford to participate once i progressed my understanding further.

[the norns shield](https://monome.org/docs/norns/shield/) was my foot in the door.
with hindsight as my guide, here are some words that would have helped me understand things quicker.

### some words
* norns is a small linux computer
* the computer does many things
* others have taught the computer many functions
* you are free to make your own functions, but you do not have to
* many of the functions, norns itself, and documentation of the aforemented are works in progress (including this)

### standing on the shoulders 
* i am lucky to have the privilege of an education which gave me time and support to understand to varying degrees: osc, midi, arduino, computer networking, and file structures
* i am also lucky to have had much support from [the norns study group discord](https://discord.com/invite/hfC5Fmw), whose members i am grateful for.

### on our feet
when my norns shield arrived in the mail, i followed [this guide](https://monome.org/docs/norns/shield/) for assembly and software, followed by [devine's tutorial](https://llllllll.co/t/norns-tutorial/23241) to get my norns shield connected to wifi. connectivity is necessary to transfer files to your norns shield, add new programs to your norns shield, and to edit the code that lives on your norns shield.

### stopping to tinker
i found devine's tutorial to be extremely helpful in learning a lot about many basic norns ideas, but wanted to understand more about the norns using the exercise i try to take in in audio environments that are new to me- building a one button sampler as simply as possible. i take this idea from the first couple microcontroller development boards i encountered, where the first program usually was to write code that makes an LED blink, and then uses a button to trigger an LED going on and off.

directly after i started planning the high-minded idea, i realized that i could use this as a chance to make a bongocat play the drums. 

so i did that.

## so, we do this
my project started off as [devine's lesson on interactions](https://github.com/neauoire/tutorial/blob/master/3_interaction.lua), from which i was able to discern how to read and react to button presses. by putting a png in the same norns directory as the lua script, i was able to display the png on the screen with `screen.display_png()`.
through iteration, i re-wrote the `redraw()` method to display a different image depending on which combination of buttons were pressed. 

in the `init()` function, `two` and `three` are boolean variables that both are declared to be false, which means that with no keys pressed, the script displays `catUp.png`.

```function redraw()
  screen.clear()
  
  --if both key 2 and key 3 are pressed, display the image with both paws down
  --also print that both keys are pressed
  if two and three then
    screen.display_png("/home/we/dust/code/fellllllll/catBoth.png", 0, 0)
    print('both on')
    
  --elseif key 3 is pressed, display the image with the right paw down
  elseif three and not two then
  --if mode == 1 then
    screen.display_png("/home/we/dust/code/fellllllll/catRight.png", 0, 0)
    
  --else if no key is pressed, display the image with the both paws up
  
  elseif not two and not three then
    screen.clear()
    screen.display_png("/home/we/dust/code/fellllllll/catUp.png", 0, 0)
    
  elseif two then
  --else if key 2 is pressed, display the image with the left paw down
    screen.display_png("/home/we/dust/code/fellllllll/catLeft.png", 0, 0)
  
  end
  screen.update()
end```

really, this was what i was most interested in doing. but in the spirit of using this new sound computer as a computer to produce sound, i began trying to add an audio element.

thus began my adventure in understanding softcut.

## softcut, made hard, and then easier
while softcut is perhaps not the best way of playing one-shot samples, setting it up to do so on the scale i was working at seemed exceptionally easy.
i copied the boilerplate initialization from [the first section of the softcut studies](https://monome.org/docs/norns/softcut/), and was able to make audio happen on a keypress!
however, when i returned to continue work the next day, i could not make my feline comrade produce audio unless i started the awake script before switching to my own.

after consulting the [softcut API](https://monome.org/docs/norns/api/modules/softcut.html), i was still stumped.

-- i should also describe how `key()` works somewhere in here.

unsure what was happening, i asked in the norns discord for help, and included that starting awake solved my problem.
as i was told, my issue stemmed from copying only the text from the softcut study link above, leading me to disregard an important feature in softcut: the playback rate.
by default, the playback rate of softcut is set to 0. so, softcut was indeed being triggered, but it was playing back at a rate of 0, which is inaudible.
by adding `softcut.rate(1, 1)` to my `function init()`, i was able once again to output audio.

## takeaways

then, i wrote a 
before i began programming, i thought out a list of things i would like my script to do. the list was
- [ ] press a button, make a drum sound
- [ ] left and right paw make different sound
- [ ] display a different image when a sound is triggered

TODO
- [ ] make a button press display text
- [ ] put one drum on softcut
- [ ] make a button press play softcut
