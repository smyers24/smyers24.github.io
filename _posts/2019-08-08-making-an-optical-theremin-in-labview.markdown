---
layout: post
title:  "Making an Optical Theremin in LabVIEW (With Autotune!)"
date:   2019-08-08 17:00:00 -0500
categories: technical
---

## INTRODUCTION
For a school project several years ago we were tasked with making a program that could run an optical theremin using LabVIEW. If you aren't familiar with what a theremin is, I'd highly recommend [reading more](https://electronics.howstuffworks.com/gadgets/audio-music/theremin.htm/printable) about them, they're really interesting! An optical theremin uses the same principal as a normal theremin but uses light sensors instead of proximity sensors. 
The basis of this project was that we were provided with the hardware setup and we had to write the code to process it. The hardware side of things was very simple: two photodiodes (detects light and generates a voltage), a chip to pass the data, and a cable to pass it to a computer. One photodiode was to be used for frequency of the tone and the other would control the amplitude, or overall volume. 
As touched on previously, this program had to be written using LabVIEW. I've mentioned this on other portions of the website, but I feel strongly about it: LabVIEW is a valid programming method. Some people discount it because "it's just pictures" and it"it's not real programming." Wrong! You design algorithms, debug, and all that other fun stuff. It's just another layer of abstraction, where the lower level stuff is just hidden. I digress.
The picture below is the first stage of the program. It handles the input from the hardware and begins processing it.
![Optical Theremin in LabVIEW - Input Stage](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_input.png)
The "DAQ Assistant" block is an NI(National Instruments) pre-made VI(virtual instrument) that manages hardware inputs and outputs. That signal is broken up because of the two indepdent photodiodes. Each signal is indepdently averaged to help accomodate for noise. The two indepdent signals are then each processed and scaled depending on the user's input. This input, for both frequency and amplitude, was meant to normalize the signals to a certain range. So if the user wanted the amplitude to be at 50% power between a relative range of 0-4, then they could do it here. See the picture below. The same applies for frequency.
![Optical Theremin in LabVIEW - Scaling Stage](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_ampscale.png)
The next interesting block that you'll notice in the input stage is "Auto-Tune". This one I'm most proud of. If aren't sure how auto-tune works, click the drop-down to below to read a bit more.
<details>
<summary>Auto-Tune Basics</summary>
<br>
#### Auto-Tune
All music notes have a corresponding physical frequency. The higher the frequency, the higher the pitch that your perceives. Pretty simple, right? Most people haven't consciously thought about what those frequencies might be, so below you'll find an image with 11 (nearly) complete octaves ![Music note frequency chart](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/frequency_chart.jpg)
So where do they come from? The exact musical theory of the ratios between semi-tones (C to C#) is beyond the scope of this post. Basically, there are different ratios of fractions that determine what the frequency of the next step up is.
Octaves are little more straightforward. Look at all the frequencies for the row of 'C'. The pattern between them is that each respective octave frequency is equal to the Octave 0 frequency times 2<sup>x</sup>, where x is equal to the octave in question. So if you want to determine the frequency of a C in Octave 6 it would be 
<code>16.35 Hz * 2<sup>6</sup> = 1046.50 Hz.</code>
Which is pretty close to what we see in the chart. It's just a difference in rounding (so turns out sig. figs. do matter outside of chemistry class).
Now that we've established some music theory, here's when auto-tune comes in to play. Essentially, all that auto-tune does is coerce an input frequency to the nearest musical frequency based on what scale you have it set on.
Here's a hypothetical to help explain it. You're signing with auto-tune and enabled, with it configured to tune you to an F scale. You sing, and the pitch your vocal chords generate is at a frequency of 450 Hz. The nearest pitch to 450 Hz on an F scale is 349.23 Hz, in octave 4. So the output from the program would be a pitch with a frequency of 349.23 Hz. 
You could also tune to a chromatic scale, which is essentially all pitches. So if you were to sing and generate that same pitch, but be tuning to a chromatic scale, then it would output the closest frequency: an A in octave 4 at 440 Hz. 
</details>

***NOTE***
Resume with explaining labview autotune
![Music note frequency chart](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_autotune_frontpanel)
## SOURCES

[1] http://www.electricalelibrary.com/en/2018/08/26/arduino-tutorial-part-9-music-and-keypad/