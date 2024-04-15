# 🌳👻 Miyazaki tribute – with SVG and GSAP

A Pen created on CodePen.io. Original URL: [https://codepen.io/nerdmanship/pen/xgmBKg](https://codepen.io/nerdmanship/pen/xgmBKg).

These are the very general thoughts I had before, during and after creating this pen. It's also shared as a post [here](https://codepen.io/nerdmanship/post/miyasaki-ghosts-and-performance-trolls).

---

# Master Miyazaki

**In the Japanese folklore**, these lovable little tree spirits – _kodamas_ – are the guardians of the forest. The amazing film [Princess Mononoke](https://en.wikipedia.org/wiki/Princess_Mononoke)  (1997) by Hayao Miyazaki, depict them as ghosts that appear in the deep forest and silently observe whatever is going on.

When intrigued, they twist their oddly shaped little heads like old wind up toys and let this strange rattling sound echo thru the landscape.

![Kodamas Original Artwork](https://s3-us-west-2.amazonaws.com/s.cdpn.io/580123/kodama_original-art.png)
Miyazaki’s style often mess with my deeply rooted preconceptions and engage me emotionally beyond plot line. The way that these light, curious and kind of cheerful kodamas contrast to the dark, damp and gloomy forest simply captivates me.

A pattern that I've noticed in his style is how he uses long shots of ambitiously decorated and well-made sceneries. **By setting the smallest detail into subtle motion he resourcefully brings a larger static artwork to life**.

---

## Old tricks to new problems
Many classic animation techniques were developed to maximise visual impact while working with limited resources. As web-animators, we can learn a lot from this. We face a similar challenge.

Our resources are limited not only in terms of time and money, but also in terms of the capacity of our devices. 

Unlike conventional animation, each frame in web-animation (of this kind) renders in realtime. To create a nice and fluid motion in the browser, our phones and computers need to be powerful enough to output enough frames each second. Or rather, our graphics and animations have to be simple enough to be rendered enough times each second.

This performance constraint is both the beauty and horror of web-animation, and with it comes the need for skills and knowledge that can be foreign to many designers like me.

It can definitely feel overwhelming – but it really shouldn't.

---

## Dealing with performance anxiety
I doubt that performance optimisation have been the main motivator for many designers to start tinkering with web-animation. It's a sneaky troll lurking in the shadows of your newfound excitement, then suddenly, just when you think you've figured something out, you have to face it.

### 👹 🅱️🅾️🅾️

As designers venturing into the cold world of development, it can be daunting to find yourself lost in tech and surrounded by very experienced developers.

*"Everybody knows so much about everything, and my god, there's so much stuff to know"*. 

But don't be discouraged. Appreciate that you have spent your time developing different skills and get excited that you're about to discover something new. The developer community is the warmest you'll ever come across and there are so many smart, curious and generous people out there who are willing to share their knowledge. When you combine your past experiences with your newly acquired knowledge, that's when innovation happens.

---

My own approach, and best recommendation to animation-curious designers who are suffering from performance anxiety, is simply to acknowledge your fears, face them and develop a sense of appreciation for available solutions. The only way forward is straight through it.

Also, allow it to progress over time. The quality of our craft is of course important, but I've found that caring too much and striving for perfection can hold me back.

Here's what I'm trying to do:

+ **Stay close to the great wizards.** Follow pioneers and influencers on twitter. Watch tuts, demos and conf talks on youtube. Read articles on topics you don't fully understand yet. The point is just to become familiar with common concepts so you know what spells to google when you face trolls in the forest.
+ **Experiment a lot.** Get yourself in trouble and give yourself a good reason to troubleshoot. Hunting for solutions when you really need one is the best motivator. And to experience and solve problems first hand is a great way to learn. 
+ **Share your stuff and thoughts** on social media and forums. Ask the silly question. Be open to the generous feedback that will prove you wrong and force you to grow.

---

## Getting myself into valuable trouble
The purpose of this experiment was to bring a complex graphic to life – just like Miyazaki – and also to discover ways to become more resourceful – just like Miyazaki.

I didn't want to limit myself while creating the graphic so I ignored what I knew about potential performance killers. I wanted to see what trolls I would run in to and what new survival skills I would force myself to develop.

---

## Switching on performance mode
During the coding phase I decided to try to isolate any feature that wasn’t absolutely necessary for the core experience.

I considered the core experience to be the introduction of parallaxing layers plus the spinning heads animation. These two things could together convey my message sufficiently. I considered everything else to be bonus; great for ambience, but not absolutely necessary.

The dancing fireflies is a good example. They add life and atmosphere to the experience and I believe they can make users curious – but they don't *have to* be there.

To each of the bonus features, I created a little switch to turn it on and off.

    features = {
        // feature: on/off
        // feature: on/off
        // ...
    }

These switches allowed me to compare different settings and benchmark results both on performance and overall experience. **All switches except filters are currently on**, so mess around with them and see if you can get a decent frame rate on your system. You'll find them at the top of the javascript document. Please share any findings.

---

## Different settings for different devices
Switches like these could also be used to deliver different experiences to different devices depending on how much they are likely to handle. Any demanding feature could be switched off when serving a less powerful device.

Just to illustrate this point, I used [Modernizr](https://modernizr.com/) to detect if the user is on a touch device, and if so, switch off all fanciness. This is a very crude implementation, but I think it captures an interesting aspect of SVGs and javascript animation – you can control virtually anything.

    if (touch device) {
        // switch off everything fancy
    }

A little disclaimer: Touch devices aren't just mobiles and tablets. I can't assume that all touch devices are less powerful. It could definitely be a touch screen desktop powerful enough to handle any crazy feature.

But most are not. Given that the limited experience is good enough, I think it's a good trade-off that some powerful devices get included by this inaccurate targeting. What do you think?

---

##  Loot from the trolls
I don't have a big report to share. The biggest wins for myself was the process of doing it, the troubleshooting and all the little seeds planted in the back of my head. But I do have some thoughts.

---

Not very surprisingly, the biggest troll in the artwork was **any filter using gaussian blur**, such as blurry edges, glowy things or shadows.  Because of lack of planning, the switch that turn this feature on and off is very blunt. It just grabs any element with a filter attribute like so: `document.querySelectorAll('[filter]')`, and removes it.

But it still makes a point – it's very costly. I assume that a filter like this is less costly when its transparency and blurriness doesn't bleed onto elements that will be animated differently. Regardless, the take away for me is to use blurry filters with extreme caution. Similar effects can be achieved in a different way.

---

Not very surprisingly number two, the **number of elements** in a parallaxing layer have a big impact on fps. If I remove all vines from the artwork I notice an increase in fps compared to if they are there. However, when comparing static vines to individually animated vines I don't notice any difference.

---

Thirdly, **subtle motion need fewer fps** to look fluid than fast motion does. This is more of an animation insight than a performance insight, but good to know nevertheless. But even if the animation looks fine at a low fps, the low performance will likely cause problems elsewhere.

However, you can [throttle](https://greensock.com/docs/#/HTML5/GSAP/TweenMax/ticker/) the fps to whatever you want, i.e. old school cartoony 12 fps like so `TweenMax.ticker.fps(12);`, to achieve a certain effect or to limit the number of ongoing calculations. I haven't tried this out yet, so maybe for the next experiment.

---

The last takeaway for me is that **I will definitely keep doing complex graphics** to find the performance sweet spot. By killing the top layer of bonus features in this pen, I could pick up the fps from a sluggish 15-20 to a smooth 50-60 on my desktop. Next time I'll be closer to 60 fps before I start testing.

---

### ❤️
I hope that this post reached and inspired some curious designers to start exploring web animation and to start developing some appreciation for performance.

Please let me know if it did. It's super rewarding and it'll inpire more pens and posts on animation!

---

### Credits 
+ The ambient audio is shamelessly ripped from the original movie
+ Sound effects were crafted from files from [freesound.org](freesound.org)
+ Another million of thanks to [Blake](http://codepen.io/osublake/) for sharing insight of [GSAP modifiersPlugin](https://greensock.com/modifiersPlugin) and [Coding Math on Youtube](https://www.youtube.com/user/codingmath)

### Some people I follow:
+ [Paul Lewis](https://twitter.com/aerotwist) // Design and performance advocate on Google
+ [Sarah Drasner](https://twitter.com/sarah_edo) // Writer and influencer on SVG and performance
+ [Rachel Nabors](https://twitter.com/rachelnabors) // Web animation expert and creator of [AAW](http://damp-lake-50659.herokuapp.com/)
+ [Paul Irish](https://twitter.com/paul_irish) // Browser performance on Google Chrome
+ Got a name for the list? Please share... <3