---
layout: post
title:  "An Introduction to Hysteresis"
date:   2019-08-08 17:00:00 -0500
categories: technical topics
---


##### Foreword
I am by no means an expert in hysteresis. This posts serves many purposes for me.
1. Learn the formatting of writing a technical post
2. Practice writing technical posts in a setting less formal than a report
3. Learn more about hysteresis

It's worth mentioning now that there are **many** kinds of hysteresis in a variety of disciplines. I'll be covering those that I'm most familiar with. If you're interested in learning about more, see the [Wikipedia](https://en.wikipedia.org/wiki/Hysteresis) article.

## Introduction

First off, what is hysteresis? According to [Wikipedia](https://en.wikipedia.org/wiki/Hysteresis), "Hysteresis is the dependence of the state of a system on its history." This has many very interesting implications. In electronics, for example, this is an extremely useful physical property. One its primary uses is noise reduction, but that's just scratching the surface. I'll go into more detail about that later. 

[Wolfram Alpha](http://demonstrations.wolfram.com/CurrentVoltageCharacteristicsOfAMemristor/img/CurrentVoltageCharacteristicsOfAMemristor.png) has a great graph to that shows hysteresis. Here, we can see that the normal V=IR relationship of a resistor doesn't apply to a memristor. The current going through it is not only depedent on the voltage and resistance, but on what was happening electrically just prior. You can can see that there can be two different currents through the memristor for the same voltage.  
![Hysteresis being displayed through the IV relationship of a memristor](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/memristor_iv.png)

## Use Case

There are many situations in electronics which can make use of this property. One of the biggest reasons is noise immunity. This is almost always something that's strived for in the world of designing high-density power electronics. An example of this is a [Schmitt Trigger](https://en.wikipedia.org/wiki/Schmitt_trigger). A Schmitt Trigger is commonly used to convert an analog signal (think of a sin wave) to a digital signal (square wave). It's so effective because it uses hysteresis to block out noise and ensure that data is accurately sampled. Let's look at an example image and dive in [1].
![Schmitt trigger with noise](https://github.com/smyers24/smyers24.github.io/raw/master/_site/assets/blog_images/schmitt_withnoise.png)

I've noted 3 points of interest in the above picture. 
###### Notation 1: 
This is where noise shows up in the signal and cause the output waveform to go low. This is *almost* always an undesired behavior. There is only a single reference threshold here, and the logic is, in some crude, generic coding it would look something like
```
if (input > singleThreshold){
    output = 1;
}
else{
    output = 0;
}
```
So when that little noise blip goes below threshold, the output goes back to 0.

###### Notation 2: 
There are now two thresholds: upper and lower. When the input is greater than the upper threshold, the signal goes high. This time, however, when it drops below upper threshold it doesn't go low as it did in the previous instance. This is because of the lower threshold.

###### Notation 3: 
As seen in 'Notation 2', the lower threshold acts as another 'gate' for stopping the signal from changing. Here's how it works: With hysteresis, you have an upper and lower limit, or threshold. Your output signal only goes high once your input is greater than the upper limit. It remains high until your input is lower than the low threshold. It's that simple!  
Some rough pseudocode can be seen below. This assume that you're constantly streaming data and evaluating measuring the input:
```
input; //assumed to be a constant stream of analog data
output = 0;
threshold = 2; //because we are using a rising edge, our must threshold starts high

if (input > threshold){
    threshold = -2;
    output = 1;
    //since we have passed the upper threshold, our output goes high
    //additionally, we won't set the output low until we pass the new (low) threshold
}
elseif(input < threshold){
    output = 0;
    //while rising, don't output high. 
    threshold = 2;
    //reset the threshold to high
}
```
This is simplified, but (I think) it gets the point across. A Schmitt Trigger includes positive feedback, which is why the threshold adjusts as it is passed. I would be happy to take suggestions as to how to improve the clarity/code.

## Concluding Notes
That's a brief overlook at hysteresis and how it can be used in electronics. I hope that my usage case did a good job at showing how the hysteresis works and a rudimentary way in which one might be able to implement it with code. 
As previously mentioned, I'd be happy to see any notes, changes, or questions that you might have about the above material. 

## Sources
[1]: Schmitt Trigger image: https://howtomechatronics.com/how-it-works/electrical-engineering/schmitt-trigger/