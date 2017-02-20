# js的兼容性(去DOM MDN全面学习)


> 各浏览器的内核
> 
> 1. IE--Trident
> 1. Firefox--Gecko
> 1. Safari, Chrome---webkit

---
> **tips:**
> 如何判断浏览器是否支持某方法?
> `console.log(document.getElementsByClassName), ` 
> 如果是undefined,就是不支持
> 
> 火狐下只有用window.open()打开的窗口,才能用window.close()关掉

1. 低版本IE(9以下,9是标准的)不支持:  
	getElementsByClassName/  
	getComputedStyle(a,b)[currentStyle]/  
	数组的indexOf,forEach,trim方法/  
	addEventListener/  
	bind/  
	ajax对象(IE6用new ActiveXObject("Microsoft XMLHTTP")来代替)/  
	ajax的onload事件(IE6用onreadystatechange事件,检测readyState属性为4或者状态码为200来代替)
2. 低版本IE支持 :getElementsByTagName/call,apply


---
**getElementByClassName兼容性解决**

```javascript
	if(document.getElementsByClassName) {
		支持,正常使用
	} else {
		var all = document.getElementByTagName('*');
		for(var a = 0, a< all.length, a++) {
			if(all[i].className.split(" ").indexOf("class名")
		}
	}
```
	
**indexOf兼容性解决**

```javascript
	function indexOf(arr, str) {
		for(var i=0; i<arr.length, i++) {
			if(arr[i] === str) {
				return i;
			}
		}
		return -1;
	}
```
	
**getComputedStyle兼容性解决**

```javascript
	ele.currentStyle.width
	等价于高版本的
	getComputedStyle(ele).width/  getComputedStyle(ele,'width')
```


----

事件对象:  
 
1. **IE高版本:**  
事件对象作为事件处理函数的第一个参数
同时全局提供一个event变量(火狐没有这个全局变量)

2. **IE低版本:**  
只能通过全局的event对象

兼容处理:
ele.onclick = function(ev) {
	var e = ev||window.event
}

----
**IE低版本`add/remove EventListener`不兼容解决**
`element.attachEvent(on+evName,evfn)` //不能捕获,只能冒泡  
`element.detachEvent(on+evName,evfn)` //不能捕获,只能冒泡  
低版本的this是window,这个问题用call解决(bind也不支持)


