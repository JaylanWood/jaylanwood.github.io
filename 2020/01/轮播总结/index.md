# 轮播总结


# 轮播总结

## 轮播1 - 普通轮播

### 效果需求

![轮播1效果](https://raw.githubusercontent.com/JaylanWood/cloudimage/master/img/%E8%BD%AE%E6%92%AD1%E6%95%88%E6%9E%9C.gif)


1. 图片切换。
2. 点击按钮切换到指定图片。
3. 自动播放。
4. 鼠标移入暂停自动播放，鼠标移出继续自动播放。
5. 当前图片对应的按钮高亮红色显示。

---

### 最终代码

[GitHub](https://github.com/JaylanWood/slide1)

---

### 核心原理

![轮播1原理](https://raw.githubusercontent.com/JaylanWood/cloudimage/master/img/%E8%BD%AE%E6%92%AD1%E5%8E%9F%E7%90%86.gif)

1. 图片横排组成一个 `images`。
2. 一个窗口展示一张图片，用 `overflow:hidden` 把超出的图片隐藏掉。
3. `images` 通过 `margin-left` 或者 `transform: translate` 来改变位置。

```javascript
// 点击按钮 切换图片
function listenToButton() {
    for (let i = 0; i < buttons.length; i++) {
        $(buttons[i]).on('click', function (xxx) {
            var index2 = $(xxx.currentTarget).index()
            var n = index2 * -300
            // 切换图片
            images.css({
                transform: 'translateX(' + n + 'px)'
            })
            // 按钮高亮
            $(buttons[index2]).addClass('red')
                .siblings().removeClass('red')
            // 更改 自动点击按钮 的 index 为 被按下按钮的 i
            index = i
        })
    }
}
```

---

## 轮播2 - 跳绳的无缝轮播

### 效果需求

轮播1不是无缝的，我要做个无缝轮播。

### 最终代码

[GitHub](https://github.com/JaylanWood/slide2)

### 核心原理

![轮播2原理](https://raw.githubusercontent.com/JaylanWood/cloudimage/master/img/%E8%BD%AE%E6%92%AD2%E5%8E%9F%E7%90%86.gif)

像跳绳一样，等待、进去、出来，循环如此。

第一张进入离开区，当第一张离开后，马上又进入等待区。

当第一张离开后，第二张进入展示区。

```css
.images {
    position: relative;
}
.images>.img {
    position: absolute;
    width: 100%;
    top: 0;
    transition: all 1s
}

/* 展示区 */
.images>.img.current {
    transform: translatex(0);
    z-index: 1;
}
/* 离开区 */
.images>.img.leave {
    transform: translatex(-100%);
}
/* 等待区 */
.images>.img.enter {
    transform: translatex(100%);
}
```

```javascript
// 切换状态
function makeCurrent($node) {
    return $node.removeClass('enter leave').addClass('current')
}
function makeLeave($node) {
    return $node.removeClass('enter current').addClass('leave')
}
function makeEnter($node) {
    return $node.removeClass('current leave').addClass('enter')
}

// 自动无缝轮播
let timer = setInterval(() => {
    makeLeave(getImg(n))
        .one('transitionend', (xxx) => {
            makeEnter($(xxx.currentTarget))
        })
    makeCurrent(getImg(n + 1))
    n += 1
}, 2000);

// 初始化
function initN(startIndex) {
    n = startIndex
    $('.images>img:nth-child(' + n + ')').addClass('current')
        .siblings().addClass('enter')
}
initN(1)
```

---

## 轮播3 - 偷梁换柱的无缝轮播

### 效果需求

![轮播3效果](https://raw.githubusercontent.com/JaylanWood/cloudimage/master/img/%E8%BD%AE%E6%92%AD3%E6%95%88%E6%9E%9C.gif)

1. 首尾切换无缝

2. 上/下一张

3. 点击按钮跳转到某一张

4. 自动轮播

5. 鼠标移入（移出）实现暂停（播放）

---

### 最终代码

[Github](https://github.com/JaylanWood/slide3)

---

### 核心原理

![轮播3原理](https://raw.githubusercontent.com/JaylanWood/cloudimage/master/img/%E8%BD%AE%E6%92%AD3%E5%8E%9F%E7%90%86.gif)

先切到首尾假图，瞬间切回真图。

`真3 --> 假1 --> 真1`

```html
<img src="./img/3.png" alt="图片3">
<img src="./img/1.png" alt="图片1">
<img src="./img/2.png" alt="图片2">
<img src="./img/3.png" alt="图片3">
<img src="./img/1.png" alt="图片1">
```

```javascript
// 普通切图
function normalSlide(index, time) {
    return $imagesWrapper.css({
        transform: `translateX(${-(index+1)*300}px)`,
        transition: `all ` + time
    })
}

// 首尾切图
function fakeSlide(fakeImgIndex, index, time) {
    // 1.先切去假图
    normalSlide(fakeImgIndex, time).one('transitionend', function () {
        // 2.一旦动画完成，马上隐藏
        $imagesWrapper.hide().offset()
        // offset，重新算位置，打断浏览器的 hide 和 show 合并。
        // 3.瞬间切到真图，并显示出来
        normalSlide(index, '0s').show()
    })
}

// 根据 当前图片current 和 目标图片index，判断 普通切图 和 首尾切图
function goToSlides(index) {
    // 判断真正要去的 index
    if (index > $buttons.length - 1) {
        index = 0
    } else if (index < 0) {
        index = $buttons.length - 1
    }

    if (current === $buttons.length - 1 && index === 0) {
        let fakeImgIndex = current + 1
        fakeSlide(fakeImgIndex, index, '1s')
    } else if (current === 0 && index === $buttons.length - 1) {
        let fakeImgIndex = current - 1
        fakeSlide(fakeImgIndex, index, '1s')
    } else {
        normalSlide(index, '1s')
    }

    current = index
}
```

---

### 其他细节

---

#### 上/下一张

```javascript
goToSlides(current - 1)
goToSlides(current + 1)
```

#### 自动轮播

```javascript
function autoPlay() {
    timerID = setInterval(() => {
        goToSlides(current + 1)
    }, 2000)
}
```

#### 点击按钮跳转某一张

```javascript
function bindButtons() {
    $('.buttonsWrapper').on('click', 'button', function (xxx) {
        goToSlides($(xxx.currentTarget).index())
    })
}
```

#### 鼠标移入（移出）实现暂停（播放）

```javascript
$('.window').on('mouseenter', function () {
    stopPlay()
}).on('mouseleave', function () {
    autoPlay()
})
```

#### 把假的第一张和最后一张做出来插入images

```javascript
// 把 HTML 的假图片删掉，用 JS 来生成插入。
function makeFakeImg() {
    let $imgs = $('.imagesWrapper img')
    let fristClone = $imgs.eq(0).clone(true)
    let latClone = $imgs.eq($imgs.length - 1).clone(true)
    $('.imagesWrapper').append(fristClone).prepend(latClone);
}
```

---

## 轮播4 - Apple 产品轮播展示

[项目地址](https://github.com/JaylanWood/AppleDevice-Slideshow)

[预览地址](https://jaylanwood.github.io/AppleDevice-Slideshow/)