# water-text 水印

## 说明：使用canvas制作水印，原理: canvas画出水印内容，再用透明度来控制内容，以此达到水印的效果。

## 1.引入jq  
    `<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-1.8.3.min.js"></script>`

## 2.html内容及css样式控制  
```
    <canvas id="waterMark" width="750" height="750"></canvas>
    <p id="waterText">water Text</p>
    <p>看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果
            看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果
            看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果
            看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果
            看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果看看水印效果
    </p>
```
```
    #waterMark {
        border: 1px solid #f00;
        position: absolute;
        top: -188px;
        left: 0;
        /* transform: rotate(30deg) */
    }
    canvas {
        border: 1px solid #ff0;
        position: absolute;
        top: 0;
        left: 0;
    }
```

## 3.初识canvas   
```
    var waterImg = document.getElementById('waterMark');
    var waterText = document.getElementById('waterText');
    var ctx = waterImg.getContext('2d');
    // 线条的颜色
    ctx .fillStyle = "#ff0";
    // 画矩形
    ctx.fillRect(0, 0, 150, 75)
    // 设置透明度
    ctx.globalAlpha = .5;
    //设置字体
    ctx.font = '20px PingFangSC-regular';
    // ctx.font = '40px Arial';
    // 水印内容及画的初识位置
    ctx.fillText('Hello world', 50, 50);
    // 空心文字
    ctx.strokeText('Hello world', 100, 100);
```

## 4.水印初步实现
```
    angle = 30 * Math.PI/180;
    ctx.rotate(angle);
    ctx.globalAlpha = .3;
    var x, y;
    // 循环 实现多个水印
    for(x = 0; x <= 750; x += (100)) {
        for (y = 0; y <= 7500; y += (100)) {
            // 实现错位
            ctx.fillText('hello', x, y);
            ctx.fillText('hello', x + 50, y + 50)
        }
    }
    // 倾斜角度
    ctx.rotate(-angle);
```

## 5.密铺水印
```
    // 密铺水印
    ctx.fillStyle = '#ff0';
    ctx.globalAlpha = .3;
    ctx.font = '20px Arial';
    // ctx.font = '20px';
    angle = 30 * Math.PI/180;
    // 画布的大小， 一般为
    cvsHeight = waterImg.height * 1.4;
    cvsWidth = waterImg.width * 1.4;
    // 设置水印内容的间距
    xGap = 20 * waterText.textContent.length;
    yGap = 20 * 8;
    ctx.rotate(-angle);
    var x, y;
    for (x = -cvsWidth; x <= cvsWidth; x += (xGap)) {
        for (y = -cvsHeight; y <= cvsHeight; y += (yGap)) {
            ctx.fillText(waterText.textContent, x, y);
            ctx.fillText(waterText.textContent, x + xGap/2, y + yGap/2);
        }
    }
    ctx.rotate(angle);
```

## 6.带logo 文字的水印
```
    var img = document.getElementById("pic")

    // 密铺水印
    document.getElementById('pic').onload = function() {
        var waterTextImg = new Image();
        waterTextImg.src = './pic.png';
        ctx.fillStyle = '#ff0';
        ctx.globalAlpha = .3;
        ctx.font = '20px Arial';
        angle = 30 * Math.PI/180;
        cvsHeight = waterImg.height * 1.4;
        cvsWidth = waterImg.width * 1.4;
        xGap = 20 * waterText.textContent.length;
        yGap = 20 * 8;
        ctx.rotate(-angle);
        var x, y;
        for (x = -cvsWidth; x <= cvsWidth; x += (xGap)) {
            for (y = -cvsHeight; y <= cvsHeight; y += (yGap)) {
                // 画logo
                ctx.drawImage(waterTextImg, x - 20, y - 15, 20, 20)
                ctx.drawImage(waterTextImg, x + xGap/2 - 20, y + yGap/2 - 15, 20, 20)
                // 画文字
                ctx.fillText(waterText.textContent, x, y);
                ctx.fillText(waterText.textContent, x + xGap/2, y + yGap/2);
            }
        }
        ctx.rotate(angle);
    }
```

## 7.自适应屏幕
```
    // 上面并不能实现响应式，以下实现响应式
    function resizeCanvas () {
        waterImg.width = document.body.clientWidth;
        waterImg.height = document.body.clientHeight;
    }
    resizeCanvas()
```

### 备注：可以直接下载shuiyin-Screen.html,里面有做带logo版的水印，纯js实现
