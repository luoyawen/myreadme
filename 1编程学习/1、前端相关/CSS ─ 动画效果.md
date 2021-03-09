# CSS ─ 动画效果

[![img](https://www.kuangstudy.com/assert/images/avatar/2.jpg) Linyu_92 ](https://www.kuangstudy.com/user/8ea0dd2a8a48451e9f921764941494bd)分类：[前端](https://www.kuangstudy.com/bbs?cid=8) 浏览：40 [评论：0](https://www.kuangstudy.com/bbs/1352976738958745602#comments)[收藏](javascript:void(0);)最后修改于： 2021/01/23 22:01:47

[展开目录+](javascript:void(0);)

# 1. 声明动画效果

> [@keyframe](https://github.com/keyframe) 动画名称 ：声明动画效果

- `from` 表示起始，也可以用 0% 表示，`to` 表示结束，也可以用 100% 表示
- 数值范围 0%~100%
- 属性：
  - `animation-name` 动画名称
  - `animation-duration` 动画持续时间，默认 0，单位 s 秒 或 ms 毫秒
  - `animation-iteration-count` 动画播放次数，预设 1，infinite 无限次

```
<div id="box1"></div>
/*在box1加入套用名为box1-Animation的动画效果，持续时间2秒*/#box1 {     width:50px;     height:50px;     position:absolute;     left:0px;     background:#f00;     animation-name:boxmove; /*动画名称*/     animation-duration:2s; /*动画持续时间*/     animation-iteration-count:infinite; /*动画播放次数*/}@keyframes boxmove {     from {          left:0px;          background:#fff;     }     to {          left:100px;          background:#f00;     }}
```

[範例一](https://linyurou.github.io/LinYurou.github.io/example/ex01.html)

```
@keyframes boxmove {     0% {          left:0px;          background:#f00;     }     50% {          left:100px;          background:#000;     }     100% {          left:200px;          background:#00f;     }}
```

[範例二](https://linyurou.github.io/LinYurou.github.io/example/ex02.html)

# 2. 延迟播放时间

> **animation-delay** ： 动画延迟播放时间，默认 0，单位 s 秒 或 ms 毫秒

如果设定为「负值」，例如 -1s、-2s，不会延迟，而是快转。

```
<div id="box1"></div><hr><div id="box2"></div><hr><div id="box3"></div>
div {     width:50px;     height:50px;     position: absolute;     animation-name:boxmove;     animation-duration:5s;}#box1 {     background: red;     top:0;     /*直接跳到第2秒的位置开始移动*/     animation-delay: -2s;}#box2 {     background: green;     top:60px;     /*完全不设定 animation-delay，一开始就移动*/}#box3 {     background: blue;     top:120px;     /*慢两秒移动*/     animation-delay: 2s;}@keyframes boxmove {     from {          left:0px;     }     to {          left:200px;     }}
```

[範例三](https://linyurou.github.io/LinYurou.github.io/example/ex03.html)

# 3. 动画速度

> animation-timing-function 动画加速度函式

- **linear**：没有任何加速减速，动画从头到尾的速度是相同的

  ![img](https://www.oxxostudio.tw/img/articles/201803/css-animation-03.jpg)

- **ease、ease-in、ease-out、ease-in-out**：

  具有加速减速的动画效果，让动画更为流畅 ( ease 为默认值 )

  ![img](https://www.oxxostudio.tw/img/articles/201803/css-animation-04.jpg)

| 属性值      | 效果                                           |
| ----------- | ---------------------------------------------- |
| ease        | 默认。动画以低速开始，然后加快，在结束前变慢。 |
| ease-in     | 动画以低速开始。                               |
| ease-out    | 动画一低速结束。                               |
| ease-in-out | 动画以低速开始和结束。                         |

```
  <div id="box1">linear</div>  <div id="box2">ease</div>  <div id="box3">ease-in</div>  <div id="box4">ease-out</div>  <div id="box5">ease-in-out</div>
  div {       position: absolute;       animation-name:boxmove;       animation-duration:10s;       animation-iteration-count:infinite;              }  @keyframes boxmove {       from {            left:0px;       }       to {            left:300px;       }  }  #box1 {       animation-timing-function:linear;  }  #box2 {       animation-timing-function:ease;  }  #box3 {       animation-timing-function:ease-in;  }  #box4 {       animation-timing-function:ease-out;  }  #box5 {       animation-timing-function:ease-in-out;  }
```

[範例四](https://linyurou.github.io/LinYurou.github.io/example/ex04.html)

# 4. 动画播放前后模式

> animation-fill-mode

| 属性值    | 效果                                                         |
| --------- | ------------------------------------------------------------ |
| none      | 默认值，不论播放次数，结束后一律返回原始状态。               |
| forwards  | 结束后，保持在最后一个影格状态。                             |
| backwards | 结束后，保持在第一个影格状态。                               |
| both      | 依据动画的次数或播放方向，保持在第一个影格或最后一个影格状态。 |

```
<div id="box1">none</div><div id="box2">forwards</div><div id="box3">backwards</div><div id="box4">both</div>
div {     position: absolute;     animation-name:boxmove;     animation-duration:5s;            }@keyframes boxmove {     from {          left:0px;     }     to {          left:300px;     }}#box1 {     animation-fill-mode: none;}#box2 {     animation-fill-mode: forwards;}#box3 {     animation-fill-mode: backwards;}#box4 {     animation-fill-mode: both;}
```

[範例五](https://linyurou.github.io/LinYurou.github.io/example/ex05.html)

# 5. 播放或暂停

> animation-play-state

- running：默认值，播放动画。
- paused：暂停动画。

```
<div></div>
div {     position: absolute;     animation-name:boxmove;     animation-duration:5s;         animation-iteration-count:infinite;                    }
@keyframes boxmove {     from {          left:0px;     }     to {          left:300px;     }}div:hover {     animation-play-state:paused;}
```

[範例六](https://linyurou.github.io/LinYurou.github.io/example/ex06.html)

标签：*CSS*

版权声明：本文为博主原创文章，转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1352976738958745602](https://www.kuangstudy.com/bbs/1352976738958745602)