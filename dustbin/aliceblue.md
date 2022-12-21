# aliceblue #

aliceblue 是 Kaosu 个人博客的样式标准。

## 0. 为什么要制定 aliceblue 的标准

主要是解决 stylesheet 的嵌套问题。我曾尝试过直接给接近原生的 HTML5 格式编写 CSS，但是很快就发现了其中的问题：

1. CSS 选择器的功能并不强大。比如说，CSS 只能选择一个元素的所有后代或者所有子代，前者会影响嵌套后代的样式（比方说，在文章的中央插入一个样式独立的投票栏），而后者则会对于处理像 &lt;blockquote&gt; 这样的递归元素显得乏力。
2. CSS 选择器嵌套规则繁琐，难以手动编写；然而使用 sass 或者 less 这样的工具会使得编译出的 css 具有恐怖的体积，既不便于传输也不便于调试。举一个经典的例子：
    ```scss
    a, p, span, div {
        .class-a, .class-b, .class-c, .class-d {
            .class-a, .class-b, .class-c {
                font-size: 1rem;
            }
        }
    }
    ```
    上述代码会生成如下 css 代码：
    ```css
    a .class-a .class-a,
    a .class-a .class-b,
    a .class-a .class-c,
    a .class-b .class-a,
    a .class-b .class-b,
    a .class-b .class-c,
    a .class-c .class-a,
    a .class-c .class-b,
    a .class-c .class-c,
    a .class-d .class-a,
    a .class-d .class-b,
    a .class-d .class-c,
    p .class-a .class-a,
    p .class-a .class-b,
    p .class-a .class-c,
    p .class-b .class-a,
    p .class-b .class-b,
    p .class-b .class-c,
    p .class-c .class-a,
    p .class-c .class-b,
    p .class-c .class-c,
    p .class-d .class-a,
    p .class-d .class-b,
    p .class-d .class-c,
    span .class-a .class-a,
    span .class-a .class-b,
    span .class-a .class-c,
    span .class-b .class-a,
    span .class-b .class-b,
    span .class-b .class-c,
    span .class-c .class-a,
    span .class-c .class-b,
    span .class-c .class-c,
    span .class-d .class-a,
    span .class-d .class-b,
    span .class-d .class-c,
    div .class-a .class-a,
    div .class-a .class-b,
    div .class-a .class-c,
    div .class-b .class-a,
    div .class-b .class-b,
    div .class-b .class-c,
    div .class-c .class-a,
    div .class-c .class-b,
    div .class-c .class-c,
    div .class-d .class-a,
    div .class-d .class-b,
    div .class-d .class-c {
        font-size: 1rem;
    }
    ```

除此之外，单靠 css 还很难对文档中（尤其是 &lt;p&gt; 中的） &lt;img&gt; 元素进行很好的定位缩放，但是如果将其使用一个 &lt;div&gt; 包装起来则可以使用 `flex` 或者 `grid` 进行定位，大大减小了 css 的编写难度。

## 1. 总览

aliceblue 的样式主要分为三种：鹰架（scaffolding, abbr. scff）、文章（article, abbr. atc）和部件（widget, abbr. wg）。鹰架主要是网站全局性定位布局的样式，文章则主要是文本内容的样式，而部件主要是网站中一些具体部件（如标题栏、按钮、菜单等）的样式。这三者之间的界限并不严格，可根据具体情况灵活调整。

除此以外，aliceblue 还有不同的主题（theme），不同的主题对应一个单独的 css 文件。原则上一个单独的网页只使用一个主题。

在以下文档中，如非特殊声明，我们约定：

1. 注有 `/* required */` 的为强制性样式。
2. 注有 `/* suggested */` 的为建议样式。
3. 注有 `/* reference */` 的为参考样式。
4. 注有 `/* deprecated */` 的为不建议使用的样式。

在第一行的注释对整个代码块有效。如未有任何注释，则默认为 `/* suggested */`。

## 2. 鹰架 `ab-scff`

### `ab-scff-fullscreen`

一个占满整个屏幕的元素。

```css
.ab-scff-fullscreen {
    position: fixed;
    width: 100vw; height: 100vh;
    overflow: hidden;
}
```

### `ab-scff-block`

一个块级元素。

注：这个元素相当的无聊。

```css
/* required */
.ab-scff-block {
    display: block;
}
```

### `ab-scff-burger`，`ab-scff-ilburger`

一个块级（块级内联）元素，其子代元素全部为绝对定位的块级元素。

```css
/* required */
.ab-scff-burger {
    display: block;
}

.ab-scff-ilburger {
    display: inline-block;
}

.ab-scff-burger > *,
.ab-scff-ilburger > * {
    display: block;
    position: absolute;
}
```

```css
/* reference */
.ab-scff-burger > .top,
.ab-scff-ilburger > .top {
    z-index: 2;
}

.ab-scff-burger > .above,
.ab-scff-ilburger > .top {
    z-index: 1;
}

.ab-scff-burder > *,
.ab-scff-ilburder > *,
.ab-scff-burger > .middle,
.ab-scff-ilburger > .middle {
    z-index: 0;
}

.ab-scff-burder > .below,
.ab-scff-ilburder > .below {
    z-index: -1;
}

.ab-scff-burder > .bottom,
.ab-scff-ilburder > .below {
    z-index: -2;
}
```

### `ab-scff-columns`

等宽多列栏目。

```css
/* suggested */
.ab-scff-columns {
    display: flex;
    /* reference */
    align-items: stretch;
    /* reference */
    gap: 2em;
}

.ab-scff-columns > * {
    flex: 1;
    /* reference */
    justify-content: center;
}
```

## 附录

### i 术语表

| 中文 | English | Abbr. |
| :----: | :----: | :----: |
|     |  aliceblue   | ab  |
| 鹰架 | scaffolding | scff |
| 文章 | article     | atc  |
| 部件 | widget      | wg   |
| 主题 | theme       |      |
| 内容 | content     | ctt  |
| 展示 | display     | dsp  |
| 内联 | inline      | il   |
