# vue-component-clock
a vue component of clock

props:

```javascript
    size: { default: 250 },//size of clock 
    dialColor: { default: "#000000" },//dial color
    dialBackgroundColor: { default: "transparent" },dial background color
    dialBackgroundImage: { default: "" },dial image url 
    secondHandColor: { default: "#F3A829" }, //second hand Color
    minuteHandColor: { default: "#222222" },//minute hand Color
    hourHandColor: { default: "#222222" },//hour hand Color
    hourCorrection: { default: "+0" }, //hour hand color
    showNumerals: { default: true },// to fix timezone
    secondSetp: { default: 100 },//if secondStep gt or eq 1000 the second hand will like a quartz clock.
    smothSecondHand: { default: false }, //second hand will move smoothly
``` 
usage:
    
```html
<template>
    <clock
          size="300"
          dialColor="#55A327"
          secondHandColor="#EA315A"
          minuteHandColor="#4182DD"
          hourHandColor="#794E12"
          secondSetp="1000"
          dialBackgroundImage="static/girl.jpg"
         ></clock> 
</template>
```
![img](https://raw.githubusercontent.com/ShawnLee2018/vue-component-clock/master/demo.gif)
