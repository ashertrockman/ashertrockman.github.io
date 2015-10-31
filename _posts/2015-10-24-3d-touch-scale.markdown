---
layout: post
title:  "3D Touch: Sensitivity, Force vs. Weight"
date:   2015-10-24 23:23:18
categories: ios
---

I wanted to investigate the granularity of the new 3D Touch feature on the iPhone 6s. Would it be sensitive enough to make a heart rate monitor? Could it be used to make a scale? So I threw together an app to display touch force, and then I found a scale...

![Measuring 3D Touch force (don't want to scratch that iPhone)]({{ site.url }}/assets/iphone-6s-scale.png)

Lacking any better instruments, I took a video of the scale and phone as I slowly increased my touch pressure. Here are the results:

![Seems to be linear]({{ site.url }}/assets/force-vs-weight.png)

There seems to be, at least, a linear relationship between force and weight given ample error in my measurements. The 3D Touch pressure is given by a float in the range [0, 6.66666]. This corresponds to the weights [0g, 337g]. Each unit of pressure seems to correspond to approximately 50g.

Taking the difference of the last touch pressure with the current for each change, I discovered that the smallest granularity is 0.020833, which corresponds to approximately one gram by my prior measurements.

With proper calibration, it should be possible to use your iPhone as a gram-accurate scale. One caveat: the object being weighed must detected by the capacitative touch screen. 

The screen is not, evidently, sensitive enough to detect my pulse.  

