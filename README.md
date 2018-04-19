# 前端批量渲染数据

## 基本用法



#### 渲染数据js脚本
function timeChunk(data, fn, count = 1, wait) {
    let obj, timer;

    function start() {
        let len = Math.min(count, data.length);
        for (let i = 0; i < len; i++) {
            val = data.shift();     // 每次取出一个数据，传给fn当做值来用
            fn(val);
        }
    }

    return function() {
        timer = setInterval(function() {
            if (data.length === 0) {    // 如果数据为空了，就清空定时器
                return clearInterval(timer);
            }
            start();
        }, wait);   // 分批执行的时间间隔
    }
}

// 测试用例
let arr = [];
for (let i = 0; i < 100000; i++) {  // 这里跑了10万数据
    arr.push(i);
}
let render = timeChunk(arr, function(n) {   // n为data.shift()取到的数据
    let div = document.createElement('div');
    div.innerHTML = n;
    document.body.appendChild(div);
}, 8, 20);

render();

```

