# 彩虹画笔特效



## 说明

画笔画出的线条会自动渐变彩虹色，非常炫酷。学习 HSL 色彩空间和自动颜色循环。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>彩虹画笔</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#000;color:white}

canvas{background:#111;cursor:crosshair;border:2px solid #333}

.tools{margin:10px}

button{padding:6px 16px;border:none;border-radius:6px;cursor:pointer;margin:3px;color:white}

</style></head>

<body>

<h2>🌈 彩虹画笔</h2>

<div class="tools">

  <input type="range" id="size" min="2" max="30" value="8">

  <input type="range" id="speed" min="0.5" max="5" value="1" step="0.5">

  <button style="background:#f44336" onclick="clearCanvas()">清空</button>

  <button style="background:#2196F3" onclick="toggleStar()">✨星星模式</button>

</div>

<canvas id="canvas" width="700" height="450"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

let drawing = false, hue = 0;

let starMode = false, stars = [];



ctx.fillStyle = "#111"; ctx.fillRect(0,0,canvas.width,canvas.height);



canvas.addEventListener("mousedown", e => { drawing = true; paint(e); });

canvas.addEventListener("mousemove", e => { if(drawing){ paint(e); } });

canvas.addEventListener("mouseup", () => drawing = false);



function paint(e) {

  let size = +document.getElementById("size").value;

  let speed = +document.getElementById("speed").value;



  hue = (hue + speed) % 360;

  let color = `hsl(${hue}, 90%, 60%)`;



  let rect = canvas.getBoundingClientRect();

  let x = e.clientX - rect.left, y = e.clientY - rect.top;



  ctx.beginPath();

  ctx.arc(x, y, size, 0, Math.PI * 2);

  ctx.fillStyle = color;

  ctx.fill();



  // 星光粒子

  if (starMode && Math.random() > 0.7) {

    stars.push({x, y, color, life: 30, size});

  }

}



function toggleStar() {

  starMode = !starMode;

  if (starMode) animateStars();

}



function animateStars() {

  if (!starMode && stars.length === 0) return;

  ctx.clearRect(0,0,canvas.width,canvas.height);

  stars = stars.filter(s => s.life-- > 0).map(s => {

    s.x += (Math.random() - 0.5) * 2;

    s.y -= 0.5;

    s.size *= 0.98;

    ctx.beginPath();

    ctx.arc(s.x, s.y, s.size, 0, Math.PI*2);

    ctx.fillStyle = s.color;

    ctx.globalAlpha = s.life / 30;

    ctx.fill();

    ctx.globalAlpha = 1;

    return s;

  });

  requestAnimationFrame(animateStars);

}



function clearCanvas() {

  ctx.fillStyle = "#111"; ctx.fillRect(0,0,canvas.width,canvas.height);

  stars = [];

}

</script></body></html>

```

