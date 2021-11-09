---
title: A Gentle Introduction to Color Math
subtitle:  Part 3, converting between color spaces.
cover-img: /assets/img/color/rainbow.jpg
thumbnail-img: /assets/img/color/rgbcolors.jpg
share-img: /assets/img/color/rgbcolors.jpg
tags: [color_perception,color_math,color_science]
---

Did you read [part 1]({% post_url 2021-11-04-A-Gentle-Introduction-to-Color-Math-Part1 %}) and [part 2]({% post_url 2021-11-04-A-Gentle-Introduction-to-Color-Math-Part2 %})? No?  If you want to understand _why_ what I'm about to show works go look at those.  If you don't really care much, skip them.

#### At long last, color math.


For this post, we'll be using the colormath library of python [docs here](https://python-colormath.readthedocs.io/en/latest/).[^1]

Here are my import statments:

``` python
from colormath.color_objects import SpectralColor, LabColor, sRGBColor,HSVColor,CMYKColor, AdobeRGBColor
import numpy as np
import seaborn as sns
from colormath.color_conversions import convert_color
from colormath.color_diff import delta_e_cie2000 as dE
``` 

Let's start with a color I use in my website:  "#006373"

It looks like this:[^2]

![Dark Cyan](https://www.colorhexa.com/006373.png)


The color I'm "thinking of" (see color is in the mind in [part 1]({% post_url 2021-11-04-A-Gentle-Introduction-to-Color-Math-Part1 %} )) is in the sRGB color space (see [part 2]({% post_url 2021-11-04-A-Gentle-Introduction-to-Color-Math-Part2 %})) because I chose that color based on how it looks to me on my computer screen.  

So let's start by creating an sRGBColor object with these values: [^3]

``` python
#in
sRGB_dark_cyan = sRGBColor(00,99,115,is_upscaled=True)
sRGB_dark_cyan
#out
sRGBColor(rgb_r=0.0,rgb_g=0.38823529411764707,rgb_b=0.45098039215686275)
```

Let's transform this into a couple of different spaces and see what happens:

```python
#in
convert_color(sRGB_dark_cyan,HSVColor)
#out
HSVColor(hsv_h=188.34782608695653,hsv_s=1.0,hsv_v=0.45098039215686275)
```

So we've got a hue angle of 188.34 (greener than I would have guessed!), full saturation, and a brightness of about 50%.  

What is this in L\*a\*b\*?

``` python
#in
sLAB = convert_color(sRGB_dark_cyan,LabColor)
sLAB
#out
LabColor(lab_l=38.129340377053886,lab_a=-18.329674989085692,lab_b=-15.993932443136504)
```
 Negative a\* values indicate green (rather than red) and negative b\* values mean blue (rather than yellow).  Yup, makes sense.


But remember when I said back in [part 2]({% post_url 2021-11-04-A-Gentle-Introduction-to-Color-Math-Part2 %}) that RGB is a _color model_ and not a color space and there are more than one RGB color spaces?  Let's do the same transforms starting with an _Adobe_ RGB Color.

``` python
#in
adRGB_dark_cyan = AdobeRGBColor(0,99,115,is_upscaled=True)
convert_color(adRGB_dark_cyan,HSVColor)
#out
HSVColor(hsv_h=188.34782608695653,hsv_s=1.0,hsv_v=0.45098039215686275)
```

HSV is the same (it's just a transform of RGB after all), but...

``` python
#in
adLAB = convert_color(adRGB_dark_cyan,LabColor)
adLAB
#out
LabColor(lab_l=36.23609478385849,lab_a=-30.839544953553734,lab_b=-19.85364047682694)
```
The lab values are different.  This is because the mapping between sRGB and Lab is different from AdobeRGB to Lab.

How different?

```python
#in
dE(sLAB, adLAB)
#out
6.230696363785524
```

That's not huge, but a reference card would definitely show the difference for someone with full color vision.

What happens if we go back?
```python
#in
convert_color(sLAB,AdobeRGBColor).get_rgb_hex() #use the lab values we got from sRGB to convert to AdobeRGBColor
#out
'#386372'
```
Whoa!  Instead of having _zero_ red, it has 21% red.

Here they are side by side[^2]:

![Dark Cyan](https://www.colorhexa.com/006373.png)
![](https://www.colorhexa.com/386372.png)

Not super different on your screen or mine, I can definitely tell the difference.

What about going from the AdobeRGBcolor -> LabColor -> sRGBColor

```python
#in
convert_color(adLAB,sRGBColor).get_rgb_hex() #use the lab values we got from AdobeRGB to convert to sRGB
#out
'#006374'
```

Almost exactly the same (save rounding errors in blue).  Why might this be?  

What it means to have a _smaller gamut_ is that more than one LabColor triplet maps to the same sRGB triplet.  Lab to sRGB is _lossy_.  AdobeRGB is too but not in a way that we can't tell when we're displaying in RGB.  

We have a similar problem with spectral color to Lab.

Here is one possible spectral curve that maps (approximately) to our hexcode above.

![](/assets/img/color/006672.png) 

The luminance is related to the _total area under the curve_ and the hue is related to the location and relative height of the peaks.  Which means you can have a lot of curves that can create the same Lab values by distributing the energy across the spectrum.


#### Why this might matter to you.


Our brains organize color in one way, displays another way.  By knowing how to convert in between color spaces, you can adjust in one that seems more intuitive to you (HSV or Lab) and then transform to the one your style sheet understands.  By looking at L\*a\*b\* values you can measure how different one color choice might be to another. 

I hope this helped you!  Let me know if it did.

<hr>


#### Footnotes
[^1]: Is there math for all of this?  Yes.  Do I want to go into it?  No.  It's been implemented for me.  But if you want to look into it, [knock yourself out](http://www.brucelindbloom.com/index.html?Math.html)
[^2]:(h/t) [https://www.colorhexa.com/](https://www.colorhexa.com/)
[^3]: We're using the native illuminant 'd65' (which is pretty close to daylight) rather than try to poke at the defaults.
