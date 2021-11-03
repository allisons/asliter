---
title: My Top 5 Favorite Things About Seaborn
subtitle:  Seaborn makes being beautiful seem effortless
cover-img: /assets/img/output_14_1.png
thumbnail-img: /assets/img/seaborn/output_14_1.png
share-img: /assets/img/seaborn/output_14_1.png
tags: [seaborn,data_presentation,matplotlib]

---

At a recent [PyData PDX meetup](https://www.meetup.com/PyData-PDX/), I presented on the 5 things I like about seaborn the most.

Most of the time when I make figures for my work, I use matplotlib.  When I tell people that, they're often baffled, skeptical, or just think I'm weird.  But I like matplotlib - it's low level, you can adjust a lot of things with it, make custom figures.  I even used it to layout the table apron for the standing desk I built.  But I still prefer to use Seaborn for a few things.  I want to outline those for you now.



```python
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
import pandas as pd

data = sns.load_dataset('penguins')
```

## Number 5:  Setting Style/Context

Really, the aesthetics of your figures are the results of style sheets.  Seaborn comes loaded up with a few "contexts" and "styles" that can change the global settings for things like background color, visible ticks in your figure, font size, and the like.  Here are some examples of the four main styles:


```python
for style in ['darkgrid','whitegrid','dark','white']:
    with sns.axes_style(style): 
        fig, ax = plt.subplots(figsize=(3.5,3.5))
        fig.suptitle(style)
        sns.barplot(x='island',y='body_mass_g', data=data)
```


![png](/assets/img/seaborn/output_2_0.png)



![png](/assets/img/seaborn/output_2_1.png)



![png](/assets/img/seaborn/output_2_2.png)



![png](/assets/img/seaborn/output_2_3.png)


I like whitegrid the best so we'll set rest of the figures to whitegrid:

```python
sns.set_style("whitegrid")
```

Next, there's a "context" setting - is this figure going to be in a paper, a jupyter notebook, a talk, or on a poster?  All of those have impact on the relative font sizes you want to use.  Here are those examples.

```python
for context in ["paper", "notebook", "talk", "poster"]:
    with sns.plotting_context(context): 
        fig, ax = plt.subplots()
        fig.suptitle(context)
        sns.barplot(x='island',y='body_mass_g', data=data)
```


![png](/assets/img/seaborn/output_4_0.png)



![png](/assets/img/seaborn/output_4_1.png)



![png](/assets/img/seaborn/output_4_2.png)



![png](/assets/img/seaborn/output_4_3.png)

We're going to set ours to notebook since that's where these are being seen from.


```python
sns.set_context('notebook')
```

## Number 4:  The Palplot

Coming in at number 4 is the "palplot".  I hear lots of argument around color palettes, the right ones to use, what not to use - but in the end, color palette is primarily a design question and you want to make your figures _pretty_.  It may seem trivial, but [studies have shown](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5573739/) being happy and having positive emotions actually helps you learn more complex concepts.  

That's where the palplot comes in.  It allows you to load up a sample color palette and just see how the colors look together. 

This is the default matplotlib palette
```python
sns.palplot(sns.color_palette())
```

![png](/assets/img/seaborn/output_7_0.png)


A color palette can be entered as just a list of hexcode strings - remember to put the "#" ahead of it.
```python
palette = ["#003f5c","#58508d","#bc5090","#ff6361","#ffa600"]
sns.palplot(palette)
```

![png](/assets/img/seaborn/output_8_0.png)



```python
palette = ["#fffcf2","#ccc5b9","#403d39","#252422","#eb5e28"]
sns.palplot(palette)
```

![png](/assets/img/seaborn/output_9_0.png)


```python
palette = ["#e63946","#f1faee","#a8dadc","#457b9d","#1d3557"]
sns.palplot(palette)
```

![png](/assets/img/seaborn/output_10_0.png)


I like to use the colors provided to me by the marketing department of my company so that I know when they inevitably take my figures entirely out of context and drop them in a presentation, at least the colors won't clash.

```python
company_colors = ['#005A96', '#FF9E15','#8fc53c',"#41b6e6",'#cb6015','#2c704F',]
sns.palplot(company_colors)
sns.set_palette(company_colors)
```

![png](/assets/img/seaborn/output_11_0.png)


## Number 3 Joint Plots

Coming in at number 3 is joint plots.  I generally eschew pre-designed figures and prefer to build mine from the ground up but jointplots just have _so much going on_ that it would be annoying to roll my own and they're pretty awesome just as is.

They can show you the distribution of variables.

```python
sns.jointplot(data=data, x="bill_length_mm", y="bill_depth_mm")
```

![png](/assets/img/seaborn/output_13_1.png)


If you have a third facet that's categorical, you can see those distributions separately.
```python
sns.jointplot(data=data, x="bill_length_mm", y="bill_depth_mm", hue="species")
```

![png](/assets/img/seaborn/output_14_1.png)


My personal favorite is kernel density estimation contour plots.

```python
sns.jointplot(data=data, x="bill_length_mm", y="bill_depth_mm", hue="species", kind="kde")
```


![png](/assets/img/seaborn/output_15_1.png)

It can even do a regression plot for you complete with slope/intercept confidence intervals based on a bootstrap analysis of your data.


```python
sns.jointplot(data=data, x="bill_length_mm", y="bill_depth_mm", kind="reg")
```

![png](/assets/img/seaborn/output_16_1.png)


## Number 2:  How Easy it is To Add Facets and Change Them

Number 2 is just how easy it is to change facets.  Most of my figure blocks end up being 10+ lines long, because I'm altering the aesthetics, adding annotation, making sure all the labels are right, but especially for just exploratory data analysis, I really like seaborn's ability to just pass a string into the dimension you want.


```python
sns.barplot(x='island',y='flipper_length_mm',data=data,)
```


![png](/assets/img/seaborn/output_18_1.png)



Go ahead, throw on another facet.

```python
sns.boxplot(x='island',y='flipper_length_mm',data=data,hue='sex')
```


![png](/assets/img/seaborn/output_19_1.png)


Oh, wait, let's swap the x and the hue facets.

```python
sns.boxplot(hue='island',y='flipper_length_mm',data=data,x='sex')
```

![png](/assets/img/seaborn/output_20_1.png)


How about we make 'em horizontal?

```python
sns.boxplot(hue='island',x='flipper_length_mm',data=data,y='sex')
```

![png](/assets/img/seaborn/output_21_1.png)

You may still want to adjust size, labels, etc but you can do so just by calling the sns axes figure and telling which axis to put itself on. 

```python
fig, ax = plt.subplots()
sns.boxplot(hue='island',x='flipper_length_mm',data=data,y='sex',ax=ax)

```

## My number one favorite thing about seaborn:  The XKCD name -> hex color dictionary

Randall Monroe of [xkcd fame](https://xkcd.com/) ran a survey a zillion years ago asking people to map names onto various RGB colors.  The result is [this page](https://xkcd.com/color/rgb/)

Seaborn has taken that page and written it into a dictionary you can call from.   


```python
colors = [sns.xkcd_rgb[color] for color in ['peach','rust','sea blue','light olive','bluish green']]
sns.palplot(colors)
```


![png](/assets/img/seaborn/output_23_0.png)


This is by far my favorite thing about seaborn.  It's so silly and simple but it's much easier to remember a color by its _name_ than it's hexcode.


I hope this was as fun for you as it was for me!  