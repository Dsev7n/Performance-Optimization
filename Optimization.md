## 函数防抖
> 参考链接：https://www.deanhan.cn/js-throttle-debounce.html
> https://blog.csdn.net/qq_41882147/article/details/81227420

+ 函数防抖：你尽管触发事件，但是我一定在事件触发后N秒才执行，如果你在一个事件触发后的n秒内又触发了这个事件，那我就以新的事件的时间为准，n秒后才执行。总之，就是要等你触发完事件n秒内不再触发事件，我才执行。比如说用手指一直按住一个弹簧，它将不会弹起直到你松手为止。
+ 函数节流：函数节流是指一定时间内执行的操作只执行一次，也就是说即预先设定一个执行周期，当调用动作的时刻大于等于执行周期则执行该动作，然后进入下一个新周期，一个比较形象的例子是如果将水龙头拧紧直到水是以水滴的形式流出，那你会发现每隔一段时间，就会有一滴水流出。
    + 应用场景：如果我们现在需要做一个记录用户鼠标移动轨迹的小应用，像这种对流畅度有一定的要求的情况，再用上面的防抖函数就不合适了，如果鼠标移动速度较快，那么只有在我们每次鼠标停下来的时候才能发送当前的位置，但是如果我们每次移动都发送并记录鼠标的位置，那也是相当恐怖的，鼠标随便移动一下就会有成百上千条记录位置的请求发出去，这个时候我们就可以用到函数节流，节流顾名思义也就是个一段儿时间去发送一次位置，可以大大减少请求发送次数。
+ 举个例子（以坐电梯为例）：
    +函数防抖：如果有人进电梯（触发事件），那电梯将在10秒钟后出发（执行事件监听器），这时如果又有人进电梯了（在10秒内再次触发该事件），我们又得等10秒再出发（重新计时）。 
    + 函数节流 ：保证如果电梯第一个人进来后，10秒后准时运送一次，这个时间从第一个人上电梯开始计时，不等待，如果没有人，则不运行。
+ 防抖实现：
```
function debounce(fn, wait) {
  var timer = null;
  return function () {
      var context = this
      var args = arguments
      if (timer) {
          clearTimeout(timer);
          timer = null;
      }
      timer = setTimeout(function () {
          fn.apply(context, args)
      }, wait)
  }
}

var fn = function () {
  console.log('boom')
}
```
+ 节流实现
```
function throttle(fn, gapTime) {
  let _lastTime = null;

  return function () {
    let _nowTime = + new Date()
    if (_nowTime - _lastTime > gapTime || !_lastTime) {
      fn();
      _lastTime = _nowTime
    }
  }
}

let fn = ()=>{
  console.log('boom')
}
```
