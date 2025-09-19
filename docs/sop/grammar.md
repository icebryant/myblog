---
top: 2
---
# markdown 语法学习记录
Markdown 是一种轻量级的标记语言（markup language），由 John Gruber（1973～）与 Aaron Swartz（1986～2013）于 2004 年创造，被网站用于编写说明文件（README）、技术文档或在论坛上发布信息。由于其语法简单，易于读写，且编写出的作品简洁美观，目前也被越来越多的人群用于日常写作、发布电子书甚至书写电子邮件。可以说，Markdown 是极简主义（minimalism）的代表作品。

简单说，Markdown 有如下优势：

语法简单，易于学习
简洁美观，可读性好
兼容 HTML/CSS，可添加丰富的样式
易于部署
1. 通用格式

1.1. 标题
# 这是一级标题111

## 这是二级标题

### 这是三级标题

#### 这是四级标题

##### 这是五级标题
1.2. 字体
**这是加粗的文字**
这是加粗的文字

*这是倾斜的文字*
这是倾斜的文字

***这是斜体加粗的文字***
这是斜体加粗的文字

~~这是加删除线的文字~~
~~这是加删除线的文字~~

1.3. 分割线
---
---

---

---
1.4. 引用
在引用的文字前加>即可
引用也可嵌套，如>>，>>>
> 这是引用的内容
>> 这是引用的内容
>>> 这是引用的内容
2. 代码、公式
2.1. 行内代码
这是`return a + b`。
这是`const test:string = '测试'`。

2.2. 代码块
```typescript
/**
 * 使用Haversine公式计算两点之间的距离（单位：米）
 * @param origin 基准点坐标
 * @param target 目标点坐标
 * @returns 两点之间的距离(米)
 */
const calculateDistance = (origin: Coordinate, target: Coordinate): number => {
    const R = 6371000; // 地球半径（米）

    const lat1 = toRadians(origin.latitude);
    const lon1 = toRadians(origin.longitude);
    const lat2 = toRadians(target.latitude);
    const lon2 = toRadians(target.longitude);

    // 计算纬度和经度的差值
    const dLat = lat2 - lat1;
    const dLon = lon2 - lon1;

    // Haversine公式
    const a =
        Math.sin(dLat / 2) * Math.sin(dLat / 2) +
        Math.cos(lat1) * Math.cos(lat2) *
        Math.sin(dLon / 2) * Math.sin(dLon / 2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));

    // 返回距离（米）
    return R * c;
};
```
2.3. 公式
行内
这是行内公式 $\Gamma(n) = (n-1)! \quad \forall n \in \mathbb{N}$
这是行内公式 $\Gamma(n) = (n-1)! \quad \forall n \in \mathbb{N}$。

块级
$$
x = \frac{-b ± \sqrt{b^2 - 4ac}}{2a}
$$

3. 列表
3.1. 无序列表
- 列表内容
3.2. 有序列表
1. 列表内容
2. 列表内容
3. 列表内容
列表内容
列表内容
列表内容
3.3. 列表嵌套
上一级和下一级之间敲3 个空格即可
- 一级无序列表内容
  - 二级无序列表内容
  - 三级无序列表内容
  - 四级无序列表内容
一级无序列表内容
二级无序列表内容
三级无序列表内容
四级无序列表内容
4. 表格
| 表头 | 表头 | 表头 |
|:----:|:----:|:----:|
| 内容 | 内容 | 内容 |
| 内容 | 内容 | 内容 |
表头	表头	表头
内容	内容	内容
内容	内容	内容
第二行分割表头和内容
文字默认居左
:-:：表示文字居中
:--：表示文字居左
--:：表示文字居右
5. 外部链接
5.1. 图片
![图片 alt](图片地址 ''图片 title'')

图片 alt 就是显示在图片下面的文字，相当于对图片内容的解释
图片 title 是图片的标题，当鼠标移到图片上时显示的内容。title 可加可不加
![知乎](https://pic2.zhimg.com/80/v2-48bbd284deacef0b5896427e660b2a51_1440w.png "知乎")



知乎


5.2. 超链接
[百度](http:/baidu.com)
百度

6. 扩展语法
基于 markdown-it。

6.1. 复选框
- [ ]
- [x]
[ ]
[x]

6.2. 高亮
==高亮==
==高亮==

6.3. 上、下标
OH^-^
KBrO~3~
OH^-^ KBrO~3~

6.4. 表情
目前，大多数的 Markdown 编辑器都支持了 emoji，其基本格式为，: 英文单词 :，如

:sunflower:
:cat:
:bike:
:icecream:
:running:
:ski:
:dog:
:mouse:
:cow:
:house:
:horse:
:sheep:

6.5. 文本绘图
Markdown 支持文本绘图，目前比较流行的有 Mermaid.js 和 PlantUML。其中，Mermaid.js 是完全 Markdown 风格的语言，可与 Markdown 文档做到无缝衔接。

作为极简主义的代表作之一，Markdown 未来的生态会越来越丰富。

7. 对接前端

7.1. HTML

7.2. JavaScript
slidev.js
reveal.js