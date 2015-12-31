---
layout: post
title:  "3D Touch: Sensitivity, Force vs. Weight"
date:   2015-10-24 23:23:18
categories: ios
---

<style>
img, video {
    width: 100%;
    border-radius: 3px;
    margin: 20px 0px;
}
table {
    width: 400px;
    margin: 50px auto;
}
.page-content a {
    font-weight: bold;
}
</style>

**[Try the web app here! TouchScale.co](http://touchscale.co)**

I wanted to investigate the granularity of the new 3D Touch feature on the iPhone 6s. Would it be sensitive enough to make a heart rate monitor? Could it be used to make a scale? So I threw together an app to display touch force, and then I found a scale...

![Measuring 3D Touch force (don't want to scratch that iPhone)]({{ site.url }}/assets/iphone-6s-scale.png)
*Measuring 3D Touch force vs. actual weight applied*

Lacking any better instruments, I took a video of the scale and phone as I slowly increased my touch pressure. Here are the results:

![Seems to be linear]({{ site.url }}/assets/force-vs-weight.png)
*Graph of force vs. weight, a straight line fits the data well*

There seems to be, at least, a linear relationship between force and weight given ample error in my measurements. The 3D Touch pressure is given by a float in the range [0, 6.66666]. This corresponds to the weights [0g, 337g]. Each unit of pressure seems to correspond to approximately 50g.

Taking the difference of the last touch pressure with the current for each change, I discovered that the smallest granularity is 0.020833, which corresponds to approximately one gram by my prior measurements.

**Update:** 0.0208333 works for light 3D Touch sensitivity. Here are values for other sensitivity settings, determined the same way:

| Sensitivity | Granularity |
| ------- | -------- |
| Light | 0.0208333 |
| Medium | 0.0166666 |
| Firm | 0.0138888 |

With proper calibration, it should be possible to use your iPhone as a gram-accurate scale. One caveat: the object being weighed must be detected by the capacitative touch screen. 

The screen is not, evidently, sensitive enough to detect my pulse.  

So by simply dividing the 3D Touch force measurement by 0.0208333, I got a fairly accurate scale. Unfortunately, the object needs to be capacitive to trigger the touch screen. Since I didn't want to scratch the screen, I settled on strawberries for my first test (which also work as excellent styluses in a pinch). 

![Weighing strawberries with iPhones]({{ site.url }}/assets/3d-touch-berries.png)
*Decently accurate so far*

Note: the strawberry is the same in both pictures. My actual scale reads 25.2g, while my 3D Touch scale reads 22.0g - a decent estimate. Doing the same thing for a 216g apple (not pictured), I got measurements between 206g and 218g depending on the apple's orientation.

You can look at the source code [here](https://github.com/ashertrockman/TouchScale).

I discovered that the force values are also accessible in Safari; you can use the `touchmove`, `touchstart`, and `touchend` events. For example: `event.touches[0].force`.  This value is the percent of the maximum force touch value; you can multiply it by the max, 6.6667, to get a result similar to UITouch.

So I used that to make a [simple web app](http://touchscale.co), TouchScale.co:

![How much does my apple weigh?]({{ site.url }}assets/touchscale.jpg)
*Excellent for weighing apples!*

This app has become quite popular; it's great to see so many people enjoying my application. I wanted to better visualize the traffic to my app, so I processed my access.log a bit and made this using the Google Maps API:

<video autoplay loop style="width: 100%;">
    <source src="{{ site.url }}assets/touchscale-hits.mp4" type="video/mp4">
</video> *Visitors to TouchScale.co over a few days*
