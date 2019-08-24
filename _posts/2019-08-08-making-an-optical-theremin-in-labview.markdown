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
![Optical Theremin in LabVIEW - Front Panel](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_frontpanel.png) 
Above is the final product of a possible front panel for this optical theremin. In the following blog post, I will go into more detail about the backend code behind it.
The picture below is the first stage of the program. It handles the input from the hardware and begins processing it.
![Optical Theremin in LabVIEW - Input Stage](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_input.png) <br>
The "DAQ Assistant" block is an NI(National Instruments) pre-made VI(virtual instrument) that manages hardware inputs and outputs. That signal is broken up because of the two indepdent photodiodes. Each signal is indepdently averaged to help accomodate for noise. The two indepdent signals are then each processed and scaled depending on the user's input. This input, for both frequency and amplitude, was meant to normalize the signals to a certain range. So if the user wanted the amplitude to be at 50% power between a relative range of 0-4, then they could do it here. See the picture below. The same applies for frequency.
![Optical Theremin in LabVIEW - Scaling Stage](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_ampscale.png)
The next interesting block that you'll notice in the input stage is "Auto-Tune". This one I'm most proud of. If aren't sure how auto-tune works, click the drop-down to below to read a bit more.
<details>
<summary>Auto-Tune Basics</summary>
<br>
<title>
Auto-Tune
</title>
All music notes have a corresponding physical frequency. The higher the frequency, the higher the pitch that your perceives. Pretty simple, right? Most people haven't consciously thought about what those frequencies might be, so below you'll find an image with 11 (nearly) complete octaves <img src="(https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/frequency_chart.jpg)" alt="Music note frequency chart">
So where do they come from? The exact musical theory of the ratios between semi-tones (C to C#) is beyond the scope of this post. Basically, there are different ratios of fractions that determine what the frequency of the next step up is.
Octaves are little more straightforward. Look at all the frequencies for the row of 'C'. The pattern between them is that each respective octave frequency is equal to the Octave 0 frequency times 2<sup>x</sup>, where x is equal to the octave in question. So if you want to determine the frequency of a C in Octave 6 it would be 
<code>16.35 Hz * 2<sup>6</sup> = 1046.50 Hz.</code>
Which is pretty close to what we see in the chart. It's just a difference in rounding (so turns out sig figs do matter outside of chemistry class).
Now that we've established some music theory, here's when auto-tune comes in to play. Essentially, all that auto-tune does is coerce an input frequency to the nearest musical frequency based on what scale you have it set on.
Here's a hypothetical to help explain it. You're singing with auto-tune enabled and have it configured to tune you to an F scale. You sing, and the pitch your vocal chords generate is at a frequency of 450 Hz. The nearest pitch to 450 Hz on an F scale is 349.23 Hz, in octave 4. So the output from the program would be a pitch with a frequency of 349.23 Hz. 
You could also tune to a chromatic scale, which is essentially all pitches. So if you were to sing and generate that same pitch, but be tuning to a chromatic scale, then it would output the closest frequency: an A in octave 4 at 440 Hz. 
</details>

The front panel for my auto tuning portion can be seen in the picture below.
<img src="(https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_autotune_frontpanel)" alt="Auto Tune front panel">
The operation is simple: Select your key, input your frequency, and enable/disable the auto tune. If you have it disabled, it passes the input through and outputs it, unmodified. If you were to choose to tune to the key of 'E', the block would look like the following picture. 
<img src="(https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_ekey.png)" alt="Auto Tune to E Key">
A chromatic auto tune might look like the following.
<img src="(https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_chromatic.png)" alt="Chromatic auto tune">
The logic behind these blocks is simple. The array on the left side is the frequency range in octave 0. The box it goes into is a case statement, and it operates similarly to a switch or case statement in other languages; In this instance, the key of E is selected. The 'N' in the upper left of the inner-most box is the number of iterations that will run, indexed to 0. So this loop will run 12 times, one for each frequency in the key. Next, the array goes into an 'Index Array' block, which outputs the value of an array at an index, in this instance index 4. The output will be 20.60, which corresponds to the key of E according to the frequency chart above. 20.60 is then multiplied by 2<sup>i</sup>, which is 0 for the first loop. <code> 20.60 * 2 <sup>0</sup> = 20.60. </code>  This is added to index 0 of an output array. The next loop then begins, and the program now computes <code> 20.60 * 2 <sup>1</sup> = 41.20. </code> This repeats, until the entire array is generated, at which point is all pushed out into a variable, 'E', and then passed to the 'Key Output'. 
The chromatic block operates using a similar premise. The notable difference, however, is that the entire array is multiplied by 2 instead of an individual index. The upward pointing arrows in that picture are <code> shift registers </code> which are used to pass values between loops. This means that the value is initialized with the Octave 0 array. The entire array is multiplied by 2 on an element-by-element basis. This doubled array is the looped back, and doubled again. This repeated for 11 total iterations. 
Next, the output frequency, whether auto tuned or not, goes into a 'Simulate Signal' block (unseen). This block simply builds a sine wave out of a frequency and an amplitude. The amplitude is chosen from the 'Amp Scale' section and the frequency is from the aforementioned block. The output of this 'Simulate Signal' block goes into the following code. 
*NOTE* The 'Simulate Signal' block is an [express VI](https://zone.ni.com/reference/en-XX/help/371361R-01/lvconcepts/expressvis/). To learn more about those, click the previous link. In essence, they're like pre-made functions in other coding languages. They are typically made by NI and many of them come with LabVIEW by default.
The last main portion of this project is the signal processing section. The output of the simulate signal block goes into the 'Filter Stage', seen in the red box in the image below. 
<img src="(https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/opticaltheremin_outputs.png)" alt="Signal processing">
The simulated sine wave enters three different types of filters: lowpass, bandpass, and highpass. These filters respectively filter our high frequency, an upper and lower bound of frequencies, and low frequency. The purpose of this is to isolate the different ranges for specific audio tuning. There is a multiplication block on each frequency band to control volume. 
Next, these filtered signals enter a measurement stage, as outlined in purple. This portion simply measures the signals intensity for the purposes of illuminating LEDs to show when a given frequency band is high. Also not the 'Volume' block, which is a master volume control. This is used to control the total volume of the theremin. 
Finally, the LED and output stage does exactly that. Digital indicators show on the front panel when the signal is high, and physical LEDs turn on at the same time. The DAQ Assistant at the end outputs the signal to a speaker. The greyed out 'Spectral Measurements' block is for generating a power spectrum graph, which was a requirement for this project. 






***NOTE***
Resume with explaining labview autotune
![Music note frequency chart]()
## SOURCES

[1] http://www.electricalelibrary.com/en/2018/08/26/arduino-tutorial-part-9-music-and-keypad/