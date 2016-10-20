# SVGA-Web-Canvas

## 最近更新

* 增加插件 SVGA-DB，使用可持久化常用 SVGA 源文件
* 优化 worker 转码
* 更新 Dynamic 功能

## 说明

[SVGA](http://code.yy.com/ued/SVGA-Format) 格式 Web Canvas 实现方式

## 实现

* 修复 [原有player](http://code.yy.com/ued/SVGAPlayer-WebCanvas) 实际使用反馈的 bug
* 轻量化，重构依赖模块，不足 100K
* 性能优化，运算开销较大 SVGA 源文件下载转码过程 迁移到 web worker，尽量避免影响主线程，造成页面卡顿
* UMD 规范，全局引用，或使用模块加载

## DEMO

[前往测试地址](http://uedfe.yypm.com/assets/lab/lijialiang/svga/)

## 使用

* 下载 build/svga.min.js & build/svga-worker.min.js
* HTML 直接外链使用 或 JS 模块 require 使用

例子

```js
let svga = new Svga({
	worker   : `${ window.location.origin }/assets/svga-worker.min.js`,
	canvas   : '#canvas',
	// assets   : `${ window.location.origin }/assets/rose.svga`,
	assets: `${ window.location.origin }/assets/kingset_dyn.svga`,
	playCount: 2,
	autoPlay : false,
	loop     : true,
}, (event) => {
	console.log('svga is ready');

	svga.setDynamicImage('../assets/avatar.png', '99');

	svga.setDynamicText({
		text  : '崔小姐不吃鱼 送了帝王套',
		size  : '30px',
		family: 'Arial',
		color : '#ffe0a4',
		key   : 'banner',
	})

	svga.play();
})

svga.complete(() => {
	console.log('svga complete');
})

svga.play();

svga.pause();

svga.stop();

```

## 参数

| 名称 | 类型 | 是否必要 | 说明 |
|-----|------|-----|---|
| worker | String | 是 | 同域 worker 文件地址 |
| canvas | String | 是 | 选择器字符串 |
| assets | String | 是 | 同域 SVGA源文件地址 |
| playCount | Number | 否 | 动画播放次数 |
| autoPlay | Boolean | 否 | 加载完成后是否自动播放 |
| loop | Boolean | 否 |是否循环播放 |
| db | Object | 否 | SvgaDB 对象 |

## 方法

```js
new Svga(...) ➜ 构造方法，返回实例对象

Svga.complete(() => { }) ➜ 动画完成后回调

Svga.play() ➜ 播放动画

Svga.pause() ➜ 暂停动画

Svga.stop() ➜ 停止动画

Svga.setDynamicImage() ➜ 设置 Dynamic 图片

Svga.setDynamicText() ➜ 设置 Dynamic 文本
```

## 插件

### SVGA-DB

通过 Web SQL Database 存储常用 SVGA 源文件，无需重复下载

#### 使用

* 下载 build/svga-db.min.js 文件
* HTML 直接外链使用 或 JS 模块 require 使用
* SvgaDB 类 在实例化时候作为 db 参数传入

例子

```js
import SvgaDB from './svga-db.min';

new Svga({
	worker   : `${ window.location.origin }/assets/svga-worker.min.js`,
	canvas   : '#canvas',
	assets   : `${ window.location.origin }/assets/rose.svga`,
	autoPlay : true,
	loop     : true,
	db 		 : SvgaDB,
})
```

## 调试构建工具

使用 [LegoFlow](http://uedfe.yypm.com/md/book/LegoFlow/) 进行调试、构建
