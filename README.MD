# HTML/CSS开发规范(for corp FE)

## 目录

1. [通用约定](#general)
	* [文档目录结构](#directory)
	* [分离](#separate)
	* [文件命名](#file-name)
	* [缩进](#indentation)
	* [编码](#encoding)
	* [小写](#lowercase)
	* [注释](#comment)
2. [HTML约定](#html)
	* [文档类型](#doctype)
	* [嵌套](#nest)
	* [标签闭合](#close-tag)
	* [格式](#format)
	* [语义化标签](#semantic)
	* [模块化](#html-module)
3. [CSS约定](#css)
	* [文件引用](#link)
	* [命名-组成元素](#element)
	* [命名-词汇规范](#word)
	* [命名-前缀规范](#prefix)
	* [id与class](#id)
	* [书写格式](#packaging)
	* [规则与分号](#semicolon)
	* [0与单位](#unit)
	* [0与小数](#decimal)
	* [属性书写顺序](#order)
	* [注释规范](#css-comment)
	* [模块化](#css-module)
4. [图像约定](#img)
	* [图像压缩](#img-compress)
	* [背景图](#background-image)
	* [前景图](#image)
	* [Sprite](#sprite)


<a name="general"></a>
## 通用约定

<a name="directory"></a>
### 1.文档目录结构

```
|-- 项目名
	|-- src	开发环境
		|-- html    静态页面模板目录
		|-- bgimg   背景图目录（假设有的话）
		|-- image   前景图目录（假设有的话）
		|-- font    字体目录（假设有的话）
		|-- scripts      脚本目录
		|-- styles（Yo）  样式目录
			|-- lib   基础库
				|-- core      核心代码：reset
				|-- element   元素
				|-- fragment  公用碎片
				|-- layout    布局
				|-- widget    组件
			|-- usage  项目具体实现
				|-- project 某个子项目
					|-- core      核心代码：桥接lib中的core，可以进行项目级扩展
					|-- fragment  项目公用碎片
					|-- module    模块
					|-- page      page桥接文件目录：src-list
					|-- export    page pack之后的文件目录
	|-- prd	生产环境
		|-- bgimg   背景图目录（假设有的话）
		|-- image   前景图目录（假设有的话）
		|-- font    字体目录（假设有的话）
		|-- scripts      脚本目录
		|-- styles（Yo）  样式目录
			|-- project1 子项目
				|-- index.css
				|-- login.css
				|-- and etc...
			|-- project2 子项目
				|-- index.css
				|-- login.css
				|-- and etc...
			|-- and etc...
```

> `src`, `scripts`, `styles` 三个目录是为了和现有项目保持一致，避免修改过大，所以保持不变。

> `html` 目录，用于存放前端开发做的静态页面，以备查阅、备份、review或给后端套页面。

> `bgimg`, `image`, `font` 三个目录在Qunar一般不会直接存在，因为我们有source服务器，这些资源都会在那上面管理；不过特殊情况也会有，比如一些独立的项目，没有使用source的，那么就需要遵循这样的目录划分。

> 至于 `html`, `bgimg`, `image`, `font` 这几个目录为什么没有加 `s`，主要是因为不希望大家去想某个目录是否为复数，简单点就好。

> `prd` 为生产环境目录，以 `xxx` 项目中的一个子项目 `mobile` 为例，其生产环境中的某个CSS外链大致如下：`//sitename.com/prd/styles/mobile/index.css`

<a name="separate"></a>
### 2.分离

结构（HTML）、表现（CSS）、行为分离（JavaScript）

> 将结构与表现、行为分离，保证它们之间的最小耦合，这对前期开发和后期维护都至关重要。

<a name="file-name"></a>
### 3.文件命名

* CSS模块文件，其文件名必须与模块名一致；

假定有这样一个模块：

	.m-detail { sRules; }
	.m-detail-hd { sRules; }
	.m-detail-bd { sRules; }
	.m-detail-ft { sRules; }

> 那么该模块的文件名应该为：`m-detail.css`

* CSS页面文件，其文件名必须与HTML文件名一致；

> 假定有一个HTML页面叫 `product.html`，那么其相对应的CSS页面文件名应该为：`product.css`

> 假定现在有一个 `product.html`，里面有2个模块：

	+<section class="m-list">
	+<section class="m-info">

那么开发人员能快速找到与该页面相关的3个直接CSS文件，包括：`product.css`, `m-list.css`, `m-info.css`

<a name="indentation"></a>
### 4.缩进

使用tab（4个空格宽度）来进行缩进，可以在IDE里进行设置

<a name="encoding"></a>
### 5.编码

* 以 UTF-8 无 BOM 编码作为文件格式；
* 在HTML中文档中用 `<meta charset="utf-8" />` 来指定编码；
* 为每个CSS文档显示的定义编码，在文档首行定义 `@charset "utf-8";`

> 在 Sass 中，如果文档中出现中文，却未显示定义编码，将会编译出错，为了统一各种写法，且提前规避错误几率，统一要求每个CSS文档都需要定义编码

<a name="lowercase"></a>
### 6.小写

* 所有的HTML标签必须小写；
* 所有的HTML属性必须小写；
* 所有的样式名及规则必须小写。

<a name="comment"></a>
### 7.注释

尽可能的为你的代码写上注释。解释为什么要这样写，它是新鲜的方案还是解决了什么问题。


<a name="html"></a>
## HTML约定

<a name="doctype"></a>
### 1.文档类型

* 统一使用HTML5的标准文档类型：`<!DOCTYPE html>`；

> HTML5文档类型具备前后兼容的特质，并且易记易书写

* 在文档doctype申明之前，不允许加上任何非空字符；

> 任何出现在doctype申明之前的字符都将使得你的HTML文档进入非标准模式

* 不允许添加 `<meta>` 标签强制改变文档模式。

> 避免出现不可控的问题



<a name="nest"></a>
### 2.嵌套

所有元素必须正确嵌套

* 不允许交叉；

不允许：

	<span><dfn>交叉嵌套</span></dfn>

应该：

	<span><dfn>交叉嵌套</dfn></span>

* 不允许非法的子元素嵌套。

不允许：

	<ul>
		<h3>xx列表</h3>
		<li>asdasdsdasd</li>
		<li>asdasdsdasd</li>
	</ul>

应该：

	<div>
		<h3>xx列表</h3>
		<ul>
			<li>asdasdsdasd</li>
			<li>asdasdsdasd</li>
		</ul>
	</div>

* 不推荐inline元素包含block元素；

不推荐：

	<span>
		<h1>这是一个块级h1元素</h1>
		<p>这是一个块级p元素</p>
	</span>

推荐：

	<div>
		<h1>这是一个块级h1元素</h1>
		<p>这是一个块级p元素</p>
	</div>

> 规则可参考：

> HTML4/XHTML1.0 Strict: [嵌套规则](http://www.cs.tut.fi/~jkorpela/html/strict.html)。
>
> HTML5: [嵌套规则](http://www.w3.org/TR/html5/)
>
> 举个例子，在HTML5中，a元素同时属于 Flow content, Phrasing content, Interactive content, Palpable content 4个分类，那些子元素是 phrasing 元素的元素可以是 a 的父元素，a 允许的子元素是以它的父元素允许的子元素为准，但不能包含 interactive 元素。

<a name="close-tag"></a>
### 3.标签闭合

所有标签必须闭合

不允许：

	<div>balabala...
	<link rel="stylesheet" href="*.css">

应该：

	<div>balabala...</div>
	<link rel="stylesheet" href="*.css" />

> 虽然有些标记没有要求必须关闭，但是为了避免出错的几率，要求必须全部关闭，省去判断某标记是否需要关闭的时间



<a name="format"></a>
### 4.格式

* 将每个块元素、列表元素或表格元素都放在新行；
* inline元素视情况换行，以长度不超过编辑器一屏为宜；
* 每个子元素都需要相对其父级缩进（参见[缩进约定](#indentation)）。

不推荐：

	<div><h1>asdas</h1><p>dff<em>asd</em>asda<span>sds</span>dasdasd</p></div>

推荐：

	<div>
		<h1>asdas</h1>
		<p>dff<em>asd</em>asda<span>sds</span>dasdasd</p>
	</div>

<a name="semantic"></a>
### 5.语义化标签

* 根据HTML元素的本身用途去使用它们；
* 禁止使用被废弃的用于表现的标签，如 center, font 等；
* 部分在XHTML1中被废弃的标签，在HTML5中被重新赋予了新的语义，在选用时请先弄清其语义，如:b, i, u等。

不允许：

	<p>标题</p>

应该：

	<h1>标题</h1>

> 虽然使用p标签，也可以通过CSS去定义它的外观和标题相同，但p标签本身的并不是表示标题，而是表示文本段落

> 参阅：[HTML5 Elements](http://www.w3.org/TR/html5/)

<a name="html-module"></a>
### 6.模块化

* 每个模块必须有一个模块名；
* 每个模块的基本组成部分应该一致；
* 模块的子节点类名需带上模块名（防止模块间嵌套时产生不必要的覆盖）；
* 孙辈节点无需再带模块名。

代码如：

	<section class="m-detail">
		<header class="m-detail-hd">
			<h1 class="title">模块标题</h1>
		</header>
		<div class="m-detail-bd">
			<p class="info">一些实际内容</p>
		</div>
		<footer class="m-detail-ft">
			<a href="#" class="more">更多</a>
		</footer>
	</section>

> 其中 `.m-detail-hd`, `.m-detail-bd`, `.m-detail-ft` 为可选，视具体模块情况决定是否需要抽象为这种 **头，中，尾** 的结构

<a name="css"></a>
## CSS约定

<a name="link"></a>
### 1.文件引用

* 不允许在 `<style>` 块中使用 `@import`；
* 不允许使用 `style` 属性写行内样式。

> 一般情况下，在页面中只允许使用 `<link />` 标签来引用CSS文件，

<a name="element"></a>
### 2.命名-组成元素

* 命名必须由单词、中划线①或数字组成；
* 不允许使用拼音（约定俗成的除外，如：youku, baidu），尤其是缩写的拼音、拼音与英文的混合。

不推荐：

	.xiangqing { sRules; }
	.news_list { sRules; }
	.zhuti { sRules; }

推荐：

	.detail { sRules; }
	.news-list { sRules; }
	.topic { sRules; }

> ①我们使用中划线 “-” 作为连接字符，而不是下划线 " _ "。
>
> 我们知道2种方式都有不少支持者，但 "-" 能让你少按一次shift键，并且更符合CSS原生语法，所以我们只选一种目前业内普遍使用的方式

<a name="word"></a>
### 3.命名-词汇规范

* 不依据表现形式来命名；
* 可根据内容来命名；
* 可根据功能来命名。

不推荐：

	left, right, center, red, black

推荐：

	nav, aside, news, type, search


<a name="prefix"></a>
### 4.命名-前缀规范

前缀|说明|示例
---|---|---|
g-|全局通用样式命名，前缀g全称为global，一旦修改将影响全站样式|g-mod
m-|模块命名方式|m-detail
ui-|组件命名方式|ui-selector
js-|所有用于纯交互的命名，不涉及任何样式规则。JSer拥有全部定义权限|js-switch

* 选择器必须是以某个前缀开头

不推荐：

	.info { sRules; }
	.current { sRules; }
	.news { sRules; }

> 因为这样将给我们带来不可预知的管理麻烦以及沉重的历史包袱。你永远也不会知道哪些样式名已经被用掉了，如果你是一个新人，你可能会遭遇，你每定义个样式名，都有同名的样式已存在，然后你只能是换样式名或者覆盖规则。

推荐：

	.m-detail .info { sRules; }
	.m-detail .current { sRules; }
	.m-detail .news { sRules; }

> 所有的选择器必须是以 g-, m-, ui- 等有前缀的选择符开头的，意思就是说所有的规则都必须在某个相对的作用域下才生效，尽可能减少全局污染。

js- 这种级别的className完全交由JSer自定义，但是命名的规则也可以保持跟重构一致，比如说不能使用拼音之类的

<a name="id"></a>
### 5.id与class

如果仅仅是写css样式用class

<a name="packaging"></a>
### 6.书写格式

* 选择器与大括号之间保留一个空格；
* 分号之后保留一个空格；
* 逗号之后保留一个空格；
* 所有规则需换行；
* 多组选择器之间需换行。

不推荐：

	main{
		display:inline-block;
	}
	h1,h2,h3{
		margin:0;
		background-color:rgba(0,0,0,.5);
	}

推荐：

	main {
		display: inline-block;
	}
	h1,
	h2,
	h3 {
		margin: 0;
		background-color: rgba(0, 0, 0, .5);
	}

<a name="semicolon"></a>
### 7.规则与分号

每条规则结束后都必须加上分号

不推荐：

	body {
		margin: 0;
		padding: 0;
		font-size: 14px
	}

推荐：

	body {
		margin: 0;
		padding: 0;
		font-size: 14px;
	}

<a name="unit"></a>
### 8.0与单位

如果属性值为0，则不需要为0加单位

不推荐：

	body {
		margin: 0px;
		padding: 0px;
	}

推荐：

	body {
		margin: 0;
		padding: 0;
	}

<a name="decimal"></a>
### 9.0与小数

如果是0开始的小数，前面的0可以省略不写

不推荐：

	body {
		opacity: 0.6;
		text-shadow: 1px 1px 5px rgba(0, 0, 0, 0.5);
	}

推荐：

	body {
		opacity: .6;
		text-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
	}


### 10.属性书写顺序

* 私有属性在前标准属性在后

```
.g-box {
　　　-webkit-box-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
　　　-moz-box-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
　　　-o-box-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
　　　box-shadow: 1px 1px 5px rgba(0, 0, 0, .5);
}
```

> 当有一天你的浏览器升级后，可能不再支持私有写法，那么这时写在后面的标准写法将生效，避免无法向后兼容的情况发生。

<a name="css-comment"></a>
### 11.注释规范

保持注释内容与星号之间有一个空格的距离

**普通注释（单行）**

	/* 普通注释 */

**区块注释**

	/**
	 * 模块: m-detail
	 * 描述：酒店详情模块
	 * 应用：page detail, info and etc...etc
	 */

> 有特殊作用的规则一定要有注释说明
> 应用了高级技巧的地方一定要注释说明

<a name="css-module"></a>
### 12.模块化

* 每个模块必须是一个独立的样式文件，文件名与模块名一致；
* 模块样式的选择器必须以模块名开头以作范围约定；

假定有一个模块如前文 [HTML模块化](#html-module)，那么 `m-detail.scss` 的写法大致如下：

	.m-detail {
		background: #fff;
		color: #333;
		&-hd {
			padding: 5px 10px;
			background: #eee;
			.title {
				background: #eee;
			}
		}
		&-bd {
			padding: 10px;
			.info {
				font-size: 14px;
				text-indent: 2em;
			}
		}
		&-ft {
			text-align: center;
			.more {
				color: blue;
			}
		}
	}

编译之后代码如下：

	.m-detail {
		background: #fff;
		color: #333;
	}
	.m-detail-hd {
    	padding: 5px 10px;
    	background: #eee;
    }
    .m-detail-hd .title {
    	background: #eee;
    }
	.m-detail-bd {
		padding: 10px;
	}
    .m-detail-bd .info {
		font-size: 14px;
		text-indent: 2em;
	}
	.m-detail-ft {
		text-align: center;
	}
    .m-detail-ft .more {
    	color: blue;
    }

> 任何超过3级的选择器，需要思考是否必要，是否有无歧义的，能唯一命中的更简短的写法


<a name="img"></a>
## 图像约定

<a name="img-compress"></a>
### 1.图像压缩

所有图片必须经过一定的压缩和优化才能发布

<a name="background-image"></a>
### 2.背景图

* 使用PNG格式而不是GIF格式，因为PNG格式色彩更丰富，还能提供更好的压缩比；
* 在需要兼容IE6的项目中，尽可能选择PNG8，而不是使用PNG24+滤镜。

<a name="image"></a>
### 3.前景图

* 内容图片建议使用JPG，可以拥有更好地显示效果；
* 装饰性图片使用PNG。

<a name="sprite"></a>
### 4.Sprite

* CSS Sprite是一种将数个图片合成为一张大图的技术（既可以是背景图也可以是前景图），然后通过偏移来进行图像位置选取；
* CSS Sprite可以减少http请求。
