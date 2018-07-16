###TOTOP

####返回顶部



* html
	```html
		<div class="totop" id="top">Top</div>
	```

* scss
	
	```scss
		
		html{
			font-size:375px;
		}
		
		@function r($px){
		    @return ($px/750px*1rem)*2;
		}
		
		.totop {
		    position: fixed;
		    right: r(15px);
		    bottom: r(40px);
		    width: r(50px);
		    height: r(50px);
		    text-align: center;
		    line-height: r(50px);
		    border-radius: 50%;
		    font-size: r(19px);
		    font-weight: 600;
		    color: #8d2846;
		    background-color: deepskyblue;
		    background: -webkit-linear-gradient(left top, deepskyblue, greenyellow);
		    background: linear-gradient(left top, deepskyblue, greenyellow);
		    transform: scale(0);
		    transform-origin: center;
		    transition: all 0.3s;
		    box-shadow: 2px 2px 7px darkgrey;
		}
	```
	
	
* 监听滚动条到达底部,totop出现
	```js
		function isReachBottom() {
			let getScrollTop = document.documentElement.scrollTop || document.body.scrollTop;
			let getWinHeight = document.documentElement.clientHeight || document.body.clientHeight;
			function getScrollHeight() {
				let bodyScrollHeight = 0
				let documentScrollHeight = 0
				if(document.body) {
					bodyScrollHeight = document.body.scrollHeight
				}
				if(document.documentElement) {
					documentScrollHeight = document.documentElement.scrollHeight
				}
				return(bodyScrollHeight - documentScrollHeight > 0) ? bodyScrollHeight : documentScrollHeight
			}
			const scrollTop = getScrollTop; // 获取滚动条的高度
			const winHeight = getWinHeight; // 一屏的高度
			const scrollHeight = getScrollHeight() // 获取文档总高度
			console.log(getScrollHeight())
			return scrollTop >= parseInt(scrollHeight) - winHeight
		}
		
		window.addEventListener('scroll', function(e) {
			var timer; //使用闭包，缓存变量
			return function() {
				if(timer) clearTimeout(timer);
				timer = setTimeout(function() {
					if(isReachBottom()) {
						toTop.style.transform = 'scale(1)';
					} else {
						toTop.style.transform = 'scale(0)';
					}
				}, 33)
			}
		}()) //此处() 立即调用return后面函数，形成闭包
	```



* totop点击事件

	```js
		var toTop = document.getElementById('top');
		var topTime;
		toTop.addEventListener('touchend', function() {
			var topH = document.documentElement.scrollTop || document.body.scrollTop;
			if(topH > 0) {
				topTime = setInterval(function() {
					topH = document.documentElement.scrollTop || document.body.scrollTop;
					document.body.scrollTop = document.documentElement.scrollTop -= 15;
					if(topH == 0) {
						clearInterval(topTime)
					}
				}, 1)
			}
		})
	```