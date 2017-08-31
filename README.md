# vue-scoller

用VUE的MiNIX混合，做的一个通用的Vue上拉加载更多组件。

只需要在组件里以v-bind指令的形式，引入loadMore方法，在组件中重写lodeMore方法即可。

# 大标题
```javascript
// import { getStyle } from '../../config/mUtils'
export const loadMore = {
	directives: {
		'load-more': {
			bind: (el, binding) => {
				let windowHeight = window.screen.height;
				let height;
				let setTop;
				let paddingBottom;
				let marginBottom;
				let requestFram;
				let oldScrollTop;
				let scrollEl;
				let heightEl;
				let scrollType = el.attributes.type && el.attributes.type.value;
				let scrollReduce = 2;
				if (scrollType == 2) {
					scrollEl = el;
					heightEl = el.children[0];
				} else {
					scrollEl = document.body;
					heightEl = el;
				}

				el.addEventListener('touchstart', () => {
					height = heightEl.clientHeight;
					if (scrollType == 2) {
						height = height
					}
					setTop = el.offsetTop;
					paddingBottom = getStyle(el, 'paddingBottom');
					marginBottom = getStyle(el, 'marginBottom');
				}, false)

				el.addEventListener('touchmove', () => {
					loadMore();
				}, false)

				el.addEventListener('touchend', () => {
					oldScrollTop = scrollEl.scrollTop;
					moveEnd();
				}, false)

				const moveEnd = () => {
					requestFram = requestAnimationFrame(() => {
						if (scrollEl.scrollTop != oldScrollTop) {
							oldScrollTop = scrollEl.scrollTop;
							moveEnd()
						} else {
							cancelAnimationFrame(requestFram);
							height = heightEl.clientHeight;
							loadMore();
						}
					})
				}

				const loadMore = () => {
					if (scrollEl.scrollTop + windowHeight >= height + setTop + 
					paddingBottom + marginBottom - scrollReduce) {
						binding.value();
					}
				}
			}
		}
	}
};
export const getStyle = (element, attr, NumberMode = 'int') => {
    let target;
    // scrollTop 获取方式不同，没有它不属于style，而且只有document.body才能用
    if (attr === 'scrollTop') { 
        target = element.scrollTop;
    }else if(element.currentStyle){
        target = element.currentStyle[attr]; 
    }else{ 
        target = document.defaultView.getComputedStyle(element,null)[attr]; 
    }
    //在获取 opactiy 时需要获取小数 parseFloat
    return  NumberMode == 'float'? parseFloat(target) : parseInt(target);
} 
```
  
    
## 中标题  
  中标题一般显示重点项,类似html的\<h2\><br />  
  你只要在标题下面输入------即可  
    
### 小标题  
  小标题类似html的\<h3\><br />  
  小标题的格式如下 ### 小标题<br />  
  注意#和标题字符中间要有空格  
  
### 注意!!!下面所有语法的提示我都先用小标题提醒了!!!   
  
### 单行文本框  
    这是一个单行的文本框,只要两个Tab再输入文字即可  
          
### 多行文本框    
    这是一个有多行的文本框  
    你可以写入代码等,每行文字只要输入两个Tab再输入文字即可  
    这里你可以输入一段代码  
  
### 比如我们可以在多行文本框里输入一段代码,来一个Java版本的HelloWorld吧
``` javascript
	const moveEnd = () => {
		requestFram = requestAnimationFrame(() => {
			if (scrollEl.scrollTop != oldScrollTop) {
				oldScrollTop = scrollEl.scrollTop;
				moveEnd()
			} else {
				cancelAnimationFrame(requestFram);
				height = heightEl.clientHeight;
				loadMore();
			}
		})
	}
```
### 链接  
1.[点击这里你可以链接到www.google.com](http://www.google.com)<br />  
2.[点击这里我你可以链接到我的博客](http://guoyunsky.iteye.com)<br />  
  
###只是显示图片  
![github](http://github.com/unicorn.png "github")  
  
###想点击某个图片进入一个网页,比如我想点击github的icorn然后再进入www.github.com  
[![image]](http://www.github.com/)  
[image]: http://github.com/github.png "github"  
  
### 文字被些字符包围  
> 文字被些字符包围  
>  
> 只要再文字前面加上>空格即可  
>  
> 如果你要换行的话,新起一行,输入>空格即可,后面不接文字  
> 但> 只能放在行首才有效  
  
### 文字被些字符包围,多重包围  
> 文字被些字符包围开始  
>  
> > 只要再文字前面加上>空格即可  
>  
>  > > 如果你要换行的话,新起一行,输入>空格即可,后面不接文字  
>  
> > > > 但> 只能放在行首才有效  
  
### 特殊字符处理  
有一些特殊字符如<,#等,只要在特殊字符前面加上转义字符\即可<br />  
你想换行的话其实可以直接用html标签\<br /\> 
