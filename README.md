## 引言

一开始只是给npy展示一下不用电脑，只用手机，就能当场做出一张有动画效果的、活泼灵动的生日贺卡。

Prompt：

> 大佬，你是一名专家前端工程师，精通前端页面的设计与实现，请叫我小柚。我男朋友的生日到了，请帮我实现一个网页，页面展示一张生日贺卡。要求如下：
> 1. 贺卡的中间部分需留空。
> 2. 贺卡的主题色为天蓝色。
> 3. 页面设计活泼灵动，有动画。
>   技术栈：HTML+CSS3+JavaScript。不要有任何额外依赖。你应当输出单个html文件。

给JS生成的气球添加从下往上飘的动画的Prompt：

> 大佬，请完善该代码，为createBalloons函数添加的气球添加动画，气球从屏幕下方开始，向上飘浮，直到出屏幕上方，大约耗时12秒。注意：1. createBalloons函数添加的气球的动画名为floating-upwards-balloon，与float动画无关。2. “移除超出屏幕的气球”的代码不要删。因为我希望耗时12秒左右气球飞出屏幕上方，然后它自己就会被这段代码删除。
> 请仅输出需要改动的代码。
> 代码规范：
>
> 1. 遵循最小改动原则，请勿修改与本次需求无关的代码。
> 2. Don't Repeat Yourself。重复出现2次及以上的逻辑需封装为函数。

## 随机产生气球

```js
function createBalloons() {
  const colors = ['#FF6B6B', '#4ECDC4', '#FFD166', '#6A0572', '#1A535C'];
  const container = document.body;

  for (let i = 0; i < 2; i++) {
    const balloon = document.createElement('div');
    balloon.className = 'balloon';
    balloon.style.left = `${5 + Math.random() * 90}%`;
    balloon.style.top = `${100 + Math.random() * 20}%`;
    balloon.style.background = colors[Math.floor(Math.random() * colors.length)];
    balloon.style.animation = `floating-upwards-balloon ${10 + Math.random() * 4}s linear forwards`;

    container.appendChild(balloon);

    // 移除超出屏幕的气球
    setTimeout(() => {
      balloon.remove();
    }, 15000);
  }
}

// 持续添加气球
function createBalloonsConstantly() {
  createBalloons();
  setTimeout(createBalloonsConstantly, 2000);
}

createBalloonsConstantly();
```

## 如何解决body元素整体上浮的一小段距离的bug

一开始给body设的`min-height: 100vh`，并且已经设置`overflow: hidden`。问了deepseek，拿到的关键改动如下：

```css
html {
  overflow: hidden;
  height: 100%;
}

body {
  height: 100%;
}
```

分析如下：

> 所以问题可能出在：当气球出现在body的底部以下时，整个文档的高度被撑开，导致出现滚动条（即使body设置了overflow:hidden，但html的溢出行为可能没有被控制）。因此，我们可以在html和body上都设置overflow: hidden。

## 气球只在贺卡的蓝色背景部分出现

主要是需要控制`balloon.style.left`，初始代码设置的值为`balloon.style.left = `${5 + Math.random() * 90}%`;`。TODO
