# css揭秘 chap3(形状)

标签（空格分隔）： css

---

<h3>9.自适应的椭圆</h3>
`border-radius`一些鲜为人知的真相：
1)可以单独制定水平半径和垂直半径，用斜杠分隔；
2)可以接受长度值还可以接受百分比值(基于元素的尺寸解析)；
3)`border-radius`是个简写属性，可以为元素每个角设定不同的值(左上角开始以顺时针的顺序)
4)为四个角提供完全不同的水平和垂直半径，方法是斜杠前指定1~4个值
自适应的半椭圆
```
div {
	display: inline-block;
	width: 16em;
	height: 10em;
	margin: 1em;
	background: #fb3;
	border-radius: 50% / 100% 100% 0 0;
}
```
四分之一的椭圆：

```
border-radius:100% 0 0 0;
```
