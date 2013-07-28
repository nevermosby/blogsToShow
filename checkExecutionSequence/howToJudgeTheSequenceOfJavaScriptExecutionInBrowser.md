如何判断JavaScript在浏览器中的执行顺序
=====================================

## Agenda
-	话题初衷
	
	为什么想做这个话题，理由很简单，工作时需要。目的是，想把这些看似与码字这个主要工程无关的知识点好好地理解清楚。

-	内容提要

	这个话题其实很大，涉及到浏览器的特性，以及Javascript的语言特性。所以做这个话题，相当于给自己挖了一个大坑，不怕，慢慢填起来，把它做成连载，想必也是对于自己一次不错的锻炼。
	所以，本文先从浏览器方面入手，讲的是浏览器之于Javascript执行顺序的影响。

## 浏览器对于Javascript的运行特性
-	载入马上执行
-	执行时阻塞页面后续的内容，包括页面的渲染和其他资源的下载
-	多个Javascript文件被引入时，它们会被串行地载入，并依次执行

## feature, maybe bug
由于浏览器之于Javascript有这样的特性，不会像并行下载css文件一样，下载js文件。所以当Javascript代码中有操作HTML的DOM树，并且在DOM树完全加载之前执行，就会发生浏览器报错对象找不到，这就是因为Javascript执行时，后面的HTML的加载被阻塞了，当前的DOM树还不存在Javascript想要的操作的DOM结点，程序也就报错了。

[示例一][example1.html]中将一段js代码插入到了head标签下，大家感受一下效果。

阻塞效果果然拔群，如果把js代码放在html的后面，感受一下[示例二][example2.html]。

综合上面两个例子，可以感受到浏览器之于Javascript的发生阻塞其他资源的过程，也就能理解为什么很多网站会把Javascript代码放在网页的最后面，或是利用一些技术，使Javascript在DOM完全加载后，再载入。

其实，不是所有的Javascript需要DOM完全载入后，才能运行。所以，异步载入Javascript，就是我们的首选。

## 几种主流的异步加载方法
-	动态创建DOM结点方式

	这个方式应该是用的最多的技术了。请看core logic：

		function loadjs(script_filename) {
		    var script = document.createElement('script');
		    script.setAttribute('type', 'text/javascript');
		    script.setAttribute('src', script_filename);
		    script.setAttribute('id', 'script_Id');
		    var script_id = document.getElementById('script_Id');
		    if(script_id){

		        document.getElementsByTagName('head')[0].removeChild(script_id);
		    }
		    document.getElementsByTagName('head')[0].appendChild(script);
		}
		var scriptPath = 'alert.js';
		loadjs(scriptPath);	
	
	完整例子请看[示例三][example3.html]。可以发现，通过这个方式，放在head里的Javascript代码也能在DOM完全加载完后运行。

-	按需异步加载方式

	第一种方式，解决了异步载入的问题，但是没有解决想让它按照我们的制定的时间运行的问题，解决这个问题，只需要将第一种方式绑定到某个事件上就行，比如某个页面元素的点击事件。请看[示例四][example4.html]


## 结束语
以上，简单介绍了浏览器之于Javascript执行顺序的影响，并且引入一些主流的异步加载Javascript的方法，还有很多这方面的知识，将慢慢道来。

- 示例代码均只在chrome浏览器下测试运行
- [参考链接][coolShell_javascriptLoadAndExecution]

[example1.html]: JavascriptExecution_alertInHead.html
[example2.html]: JavascriptExecution_alertAfterHtml.html
[example3.html]: JavascriptExecution_dynamicCreateDom.html
[example4.html]: JavascriptExecution_bindListener.html
[coolShell_javascriptLoadAndExecution]: http://coolshell.cn/articles/9749.html