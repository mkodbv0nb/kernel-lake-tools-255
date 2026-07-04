# 曼陀罗旋转绘画



## 说明

画一笔线，自动复制旋转出 N 重对称图案，创造出美丽的 Mandala 艺术品。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>曼陀罗旋转画</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#1a1a2e;color:white}

canvas{background:#16213e;cursor:crosshair;border-radius:50%}

.tools{margin:10px;display:flex;justify-content:center;gap:10px;flex-wrap:wrap;align-items:center}

button{padding:6px 16px;border:none;border-radius:6px;cursor:pointer}

</style></head>

<body>

<h2>🌸 曼陀罗旋转绘画</h2>

<p>你画一笔，自动旋转复制！用滑块调整花瓣数量</p>

<div class="tools">

  <span>花瓣: <span id="petalsVal">6</span></span>

  <input type="range" id="petals" min="2" max="20" value="6" oninput="document.getElementById('petalsVal').textContent=this.value">

  <input type="color" id="color" value="#ff69b4">

  <input type="range" id="size" min="1" max="10" value="3">

  <button style="background:#e74c3c;color:white" onclick="clearCanvas()">清空</button>

  <button style="background:#2ecc71;color:white" onclick="saveArt()">保存</button>

</div>

<canvas id="canvas" width="500" height="500"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

let drawing = false;

const cx = 250, cy = 250;



function getPos(e) {

  let rect = canvas.getBoundingClientRect();

  return { x: e.clientX - rect.left, y: e.clientY - rect.top };

}



canvas.addEventListener("mousedown", e => {

  drawing = true;

  let p = getPos(e);

  drawAllPetals(p.x, p.y, p.x, p.y);

});



canvas.addEventListener("mousemove", e => {

  if (!drawing) return;

  let p = getPos(e);

  drawAllPetals(p.x, p.y, p.x, p.y);

});



canvas.addEventListener("mouseup", () => drawing = false);



// 触屏

canvas.addEventListener("touchstart", e => { e.preventDefault(); drawing = true; let t=e.touches[0]; drawAllPetals(t.clientX-canvas.offsetLeft,t.clientY-canvas.offsetTop,t.clientX-canvas.offsetLeft,t.clientY-canvas.offsetTop); });

canvas.addEventListener("touchmove", e => { e.preventDefault(); if(!drawing) return; let t=e.touches[0]; drawAllPetals(t.clientX-canvas.offsetLeft,t.clientY-canvas.offsetTop,t.clientX-canvas.offsetLeft,t.clientY-canvas.offsetTop); });

canvas.addEventListener("touchend", () => drawing = false);



function drawAllPetals(x, y) {

  let petals = +document.getElementById("petals").value;

  let size = +document.getElementById("size").value;

  let color = document.getElementById("color").value;



  for (let i = 0; i < petals; i++) {

    let angle = (Math.PI * 2 / petals) * i;

    let cos = Math.cos(angle), sin = Math.sin(angle);



    // 坐标旋转变换

    let dx = x - cx, dy = y - cy;

    let rx = dx * cos - dy * sin;

    let ry = dx * sin + dy * cos;



    ctx.beginPath();

    ctx.arc(cx + rx, cy + ry, size, 0, Math.PI*2);

    ctx.fillStyle = color;

    ctx.fill();

  }

}



function clearCanvas() {

  ctx.clearRect(0, 0, 500, 500);

  ctx.fillStyle = "#16213e"; ctx.fillRect(0,0,500,500);

}



function saveArt() {

  let link = document.createElement("a");

  link.download = "mandala.png"; link.href = canvas.toDataURL(); link.click();

}

</script></body></html>

```

