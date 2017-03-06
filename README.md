解决setInterval计时器不准的问题
=============================

> 在Js中如果打算使用setInterval进行倒数，计时等功能，往往不准确，因为setInterval的回调函数并不是在到时间后立即执行，而是等计算机资源空闲下来才会执行，而下一次触发时间是在setInterval回调函数执行，setInterval的计时会越来越不准，延迟很厉害。

#### 下面的代码可以说明这个问题

```javascript

	var startTime = new Date().getTime();
	var count = 0;
	
	// 耗时任务
	setInterval(function(){
		var i = 0;
		while(i++ < 100000000);
	},0);
	
	setInterval(function(){
		count++;
		console.log(new Date().getTime() - (startTime + count * 1000));
	},1000);
	
```

> 可以看到延迟是越来越严重，为了在JS里可以使用准确的计时功能，我们可以

```js

	var startTime = new Date().getTime();
	var count = 0;
	setInterval(function(){
		var i = 0;
		while(i++ < 100000000);
	}, 0);
	
	function fixed(){
		count++;
		var offset = new Date().getTime() - (startTime + count * 1000);
		var nextTime = 1000 - offset;
		if(nextTime < 0){
			nextTime = 0;
		};
		setTimeout(fixed, nextTime);

		console.log(new Date().getTime() - (startTime + count * 1000));
	}

```

> 可以看出虽然出发时间并非绝对准确，但由于每次触发都计时修正，所以并没有曹成误差累计。