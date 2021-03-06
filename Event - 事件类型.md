## 事件类型

### UI 事件，当用户与页面上的元素交互时触发

- load: 当页面完全加载后在 window 上面触发，当所有框架都加载完毕时在框架上面触发，当图像加载完毕时在 `<img>` 元素上面触发，或者当嵌入的内容加载完毕时在 `<object>` 元素上面触发

**一般来说，在 window 上面发生的任何事件都可以在 `<body>` 元素中通过相应的特性来指定，因为在 HTML 中无法访问 window 元素**

```
// 图像加载完毕后显示一个警告框
var image = document.getElementById('myImage');
EventUtil.addHandler(image, 'load', function (event) {
	event = EventUtil.getEvent(event);
	alert(EventUtil.getTarget(event).src);
});

image.src = 'smile.gif';

// 上述例子也可以写成如下：
EventUtil.addHandler(window, 'load', function () {
	var image = new Image();
	EventUtil.addHandler(image, 'load', function (event) {
		alert('Image loaded');
	});
	image.src = 'smile.gif';
});

// 为`<script>` 元素指定事件处理程序
EventUtil.addHandler(window, 'load', function () {
	var script = document.createElement('script');
	script.type = 'text/javascript';
	script.src = 'example.js';
	EventUtil.addHandler(script, 'load', function (event) {
		alert('Loaded');
	});
	document.body.appendChild(script);
});
```

> 当图像元素指定了 src 特性，图像就开始下载，所以必须在指定 src 特性之前，为其指定 load 事件处理程序。如果是 `<script>` 元素指定了 src 特性还需要把元素添加到文档中，才开始下载 JavaScript 文件

- unload: 用户从一个页面切换到另一个页面，触发 unload 事件

- resize: 当浏览器窗口被调整到一个新的高度或宽度是，就会触发 resize 事件。这个事件在 window （窗口）上面触发，因此可以通过 JavaScript 或 <body> 元素中的 onresize 特性来指定事件处理程序
```
// JavaScript 方式：
EventUtil.addHandler(window, 'resize', function (event) {
	alert('Resized');
})
```

> 不同浏览器对 resize 事件的触发情况不一致。IE、Safari、Chrome 和 Opera 会在浏览器窗口变化了 1 像素是就触发 resize 事件，然后随着变化不断重复触发。FireFox 则是会在用户停止调整窗口大小是才会触发 resize 事件

- scroll: 文档滚动期间重复触发

```
EventUtil.addHandler(window,'scroll',function(event){
	if (document.compatMode == 'CSS1Compat'){
		console.log(document.documentElement.srcollTop);
	}else{
		console.log(document.body.scrollTop);
	}
});

// 滚动事件可以绑定在 window 、document、 document.body 上都能正常返回 scrollTop （垂直滚动距离）
```

### 焦点事件

**焦点事件会在页面元素获得或失去焦点时触发。利用这些事件并与 document.hasFocus() 方法及 document.activeElement 属性配合，可以知晓用户在页面上的行踪**

- blur: 在元素失去焦点时触发。这个时间不会冒泡，所有浏览器都支持它
- focus: 在元素获取焦点时触发。这个事件不会冒泡，所有浏览器都支持它
- focusin: 在元素获得焦点时触发。这个事件与 HTML 事件 focus 等价，但冒泡
- focusout: 在元素失去焦点时触发。这个事件与 HTML 事件 blur 等价，但冒泡
- DOMFocusIn: 在元素获取焦点时触发。这个事件与 HTML 事件 focus 等价，但冒泡，不建议使用
- DOMFocusOut: 在元素失去焦点时触发。这个事件与 HTML 事件 blur 等价，但冒泡，不建议使用

### 鼠标事件

- click: 单击
- dbclick: 双击
- mousedown: 用户按下任意鼠标按钮时触发。不能通过键盘触发这个事件
- mouseup: 在用户释放鼠标按钮时触发。不能通过键盘触发这个事件
- mousemove: 当鼠标指针在元素内部移动时重复地触发。不能通过键盘触发这个事件
- mouseout: 在鼠标指针位于元素上方，然后用户将其移入另一个元素时触发。又移入的另一个元素可能位于前一个元素的外部，也可能是这个元素的子元素。不能通过键盘触发这个事件
- mouseover: 在鼠标指针位于一个元素外部，然后用户将其首次移入另一个元素边界之内是触发。不能通过鼠标触发这个事件
- mouseenter: 在鼠标光标从元素外部首次移动到元素范围之内是触发。这个事件不冒泡，而且在光标移动到后代元素上不会触发
- mouseleave: 在位于元素上方的鼠标光标移动元素范围之外是触发。这个事件不冒泡，而且在光标移动到后代元素上不会触发

> 只有 mouseenter 和 mouseleave 事件不冒泡，mouseover 和 mouseout 都含有一个相关元素 ，通过 relatedTarget 获取

**1. 鼠标事件的事件对象含有特定的坐标属性，如下：**

- clientX: 表示事件发生时鼠标指针在视口中的水平坐标
- clientY: 表示事件发生时鼠标指针在视口中的垂直坐标
- pageX: 表示事件发生时鼠标指针离页面左边的水平距离，如果页面滚动，则包含滚动距离
- pageY: 表示事件发生时鼠标指针离页面顶部的垂直距离，如果页面滚动，则包含滚动距离
- screenX: 表示事件发生时鼠标指针在屏幕的水平坐标
- screenY: 表示事件发生时鼠标指针在屏幕的垂直坐标

**2. 修改键**

- event.shiftKey: 鼠标操作时 shift 键被按下时，为 true 值，反之为 false 值
- event.ctrlKey: 鼠标操作时 ctrl 键被按下时，为 true 值，反之为 false 值
- event.altKey: 鼠标操作时 alt 键被按下时，为 true 值，反之为 false 值
- event.metaKey: 鼠标操作时 meta 键被按下时，为 true 值，反之为 false 值

**3. 相关元素**

mouseover 和 mouseout 事件涉及一个相关元素。

- 对于 mouseover 事件而言，事件的主目标是获得光标的元素，而相关元素就是那个失去光标的元素
- 对于 mouseout 事件而言，事件的主目标是失去光标的元素，而相关元素则是获得光标的元素

> DOM 通过 event 对象的 ralatedTarget 属性提供了相关元素的信息，这个属性只对于 mouseover 和 mouseout 事件才包含的值，对于其他事件，这个属性的值是 null
> IE8 之前版本不支持 ralatedTarget 属性，在 mouseover 事件触发时，通过 fromElement 属性中的保存了相关元素；在 mouseout 事件触发时，通过 toElement 属性中保存着相关元素

```
// 获取相关元素
getRelatedTrget: function (event) {
	if (event.relatedTarget) {
		return event.relatedTarget;
	}else if (event.fromElement) {
		return event.fromElement;
	}else if (event.toElement) {
		return event.toElement;
	}else{
		return null;
	}
}
```

**4. 鼠标按钮**

DOM 的 button 属性可能有以下3个值： 0 表示主鼠标按钮，1 表示中间的鼠标按钮（鼠标滚轮按钮），2 表示次鼠标按钮

IE8 及之前的版本提供的 button 属性的值 与 DOM 的 button 属性有很多差异
- 0: 表示没有按下按钮
- 1: 表示按下了主鼠标按钮
- 2: 表示按下了次鼠标按钮
- 3: 表示同时按下了主、次鼠标按钮
- 4: 表示按下了中间的鼠标按钮
- 5: 表示同时按钮了主鼠标按钮和中间鼠标按钮
- 6: 表示同时按下了次鼠标按钮和中间的鼠标按钮
- 7: 表示同时按下了三个鼠标按钮

```
// 支持 DOM 版鼠标事件的浏览器可以通过 hasFeature() 方法来检测
fetBurron: function (event) {
	if(document.implementation.hasFeature('MouseEvents', '2.0')){
		return event.button;
	}else{
		switch(event.button){
			case 0:
			case 1:
			case 3:
			case 5:
			case 7:
				return 0;
			case 2:
			case 6:
				return 2;
			case 4:
				return 1;
		}
	}
}
```

**5. 更多的事件信息**

"DOM2 级事件"规范在 event 对象中还提供了 detail 属性，用于给出有关事件的更多信息。对于鼠标事件来说，detail 中包含了一个数值，表示在给定位置上发生了多少次单击。在同一个元素上相继发生一次 mousedown 和 一次 mouseup 事件算作一次单击。detail 属性从 1 开始计数，每次单击发生后都会递增。如果鼠标在 mousedown 和 mouseup 之间移动了位置，则 detail 会被重置为0

**6. 鼠标滚轮事件**

- mousewheel: 当用户通过鼠标滚路浓郁页面交互、在垂直方向上滚动页面时，就会触发 mousewheel 事件。该事件对应的 event 对象除包含鼠标事件的所有标准信息外，还包含了一个特殊的 wheelDelta 属性。当用户向前滚动鼠标滚轮时，wheelDelta 是 120 的倍数，向后滚动式为 -120 的倍数。

- DOMMouseScroll: 功能与 mousewheel 一样，只是该事件有关滚轮的信息则包含在 event 对象的 detail 属性中，而且向前滚动鼠标滚轮，detail 是 -3 的倍数，向后滚动时为 3 的倍数

> 这个事件在任何元素上面都能触发，最终会冒泡到 document 或 window 对象，需要注意的是这个事件的 wheelDelta 属性有些浏览器上正负号是与上述相反的

```
// 获取鼠标滚轮滚动信息
getWheelDelta: function(event) {
	if(event.wheelDelta){
		return event.wheelDelta;
	}else{
		return -event.detail * 40;
	}
}
```

**7. 触屏设备**

> 对于 iOS 和 Android 设备的实现非常特别

### 键盘与文本事件

- keydown: 当用户按下键盘上的任意键是触发，而且如果按住不放的话，会重复触发此事件

- keypress: 当用户按下键盘上的字符键是触发，而且如果按住不放的话，会重复触发此事件

- keyup: 当用户释放键盘上的键时触发

> 所有元素都支持以上3个事件，但只有在用户通过文本框输入时才最为常用

1. 键码

**当用户按下一个键盘上的字符键时，首先会触发 keydown 事件，然后紧跟着是 keypress 事件，最后会触发 keyup 事件。其中 keydown 和 keypress 都是在文本框发生变化之前被触发的，而 keyup 事件则是在文本框已经发生变化之后被触发的。如果用户按下了一个字符键不放，就会重复触发 keydown 和 keypress 事件，直到用户松开该键为止，此时会触发 keyup。如果用户按下的是一个非字符键，那么首次会触发 keydown 事件，然后就是 keyup 事件，如果按下了一个非字符键不放，此时会重复触发 keydown 事件，直到用户松开这个键，此时会触发 keyup**

> keydown 和 keyup 事件，通过 event 对象的 keyCode 属性可以获取一个代码，与键盘上的一个特定的键对应。

> 事件的 event 对象含有一个 charCode 属性，这个属性只有在发生 keypress 事件时才包含值，可以获取被按下的键对应的键码，要想跨浏览器的方式取得键码，应该先检测 charCode 是否能用再调用 keyCode（在某些浏览器上keyCode保存的字符的 ASCII 编码）

2. textInput: 只有可编辑区域才能出发 textInput 事件，该事件只会在用户按下能够输入实际字符的键时才会被触发。通过 event 对象的 data 属性能访问到用户输入的字符。例如：用户按下 S 键， data 的值是 s,如果按住上档键时按下该键， data 的值就是 'S'。

> 另外该事件的 event 对象还含有一个 inputMethod 属性，表示文档输入到文本框中的方式，支持 textInput 属性的浏览器有 IE9+、Safari 和 Chrome