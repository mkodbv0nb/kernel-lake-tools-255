# 手指画（多点触控画板）



## 说明

专为移动设备设计的画板，支持多点触控，两个手指同时画线。也可在桌面端用鼠标使用。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1">

<title>手指画画</title>

<style>

*{margin:0;padding:0;box-sizing:border-box}

body{touch-action:none;overflow:hidden;font-family:Arial}

canvas{display:block;background:white}

.tools{position:fixed;bottom:10px;left:50%;transform:translateX(-50%);display:flex;gap:8px;z-index:10;flex-wrap:wrap;justify-content:center}

.tools button,.tools input{padding:8px 16px;border:none;border-radius:20px;cursor:pointer;font-size:14px}

</style></head>

<body>

<canvas id="canvas"></canvas>

<div class="tools">

  <input type="color" id="color" value="#ff4081">

  <input type="range" id="size" min="2" max="20" value="5">

  <button style="background:#333;color:white" onclick="clearAll()">清空</button>

  <button style="background:#4CAF50;color:white" onclick="savePic()">保存</button>

</div>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");



function resize() {

  canvas.width = window.innerWidth;

  canvas.height = window.innerHeight;

}

resize(); window.addEventListener("resize", resize);



// 追踪多个手指

const touches = {};



canvas.addEventListener("touchstart", e => {

  e.preventDefault();

  for (let t of e.changedTouches) {

    touches[t.identifier] = { x: t.clientX, y: t.clientY };

    drawDot(t.clientX, t.clientY);

  }

});



canvas.addEventListener("touchmove", e => {

  e.preventDefault();

  for (let t of e.changedTouches) {

    let prev = touches[t.identifier];

    if (prev) {

      drawLine(prev.x, prev.y, t.clientX, t.clientY);

      prev.x = t.clientX;

      prev.y = t.clientY;

    }

  }

});



canvas.addEventListener("touchend", e => {

  for (let t of e.changedTouches) {

    delete touches[t.identifier];

  }

});



// 桌面鼠标支持

let mousedown = false;

canvas.addEventListener("mousedown", e => { mousedown = true; drawDot(e.offsetX, e.offsetY); });

canvas.addEventListener("mousemove", e => {

  if (!mousedown) return;

  ctx.lineTo(e.offsetX, e.offsetY); ctx.stroke();

});

canvas.addEventListener("mouseup", () => { mousedown = false; ctx.closePath(); });



function drawDot(x, y) {

  ctx.beginPath(); ctx.arc(x, y, getSize()/2, 0, Math.PI*2);

  ctx.fillStyle = getColor(); ctx.fill();

}



function drawLine(x1, y1, x2, y2) {

  ctx.beginPath(); ctx.moveTo(x1, y1); ctx.lineTo(x2, y2);

  ctx.strokeStyle = getColor(); ctx.lineWidth = getSize();

  ctx.lineCap = "round"; ctx.stroke();

}



function getColor() { return document.getElementById("color").value; }

function getSize() { return +document.getElementById("size").value; }



function clearAll() { ctx.clearRect(0,0,canvas.width,canvas.height); }

function savePic() {

  let a = document.createElement("a");

  a.download = "finger_paint.png"; a.href = canvas.toDataURL(); a.click();

}

</script></body></html>

```

