# 散布粒子画板



## 说明

点击画布随机散布彩色粒子。可调节粒子数量、大小、扩散半径。模拟喷漆枪效果。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>粒子喷画</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#1a1a1a;color:white}

canvas{background:black;cursor:crosshair;border:2px solid #444}

.tools{display:flex;justify-content:center;gap:10px;align-items:center;flex-wrap:wrap;margin:10px}

button{padding:6px 14px;border:none;border-radius:6px;cursor:pointer}

</style></head>

<body>

<h2>💥 粒子喷画</h2>

<div class="tools">

  <input type="color" id="color" value="#ff4081">

  <span>数量:</span><input type="range" id="count" min="5" max="100" value="30">

  <span>散布:</span><input type="range" id="spread" min="10" max="100" value="40">

  <span>大小:</span><input type="range" id="psize" min="1" max="8" value="2">

  <button style="background:#e91e63;color:white" onclick="clearCanvas()">清空</button>

  <button style="background:#00bcd4;color:white" onclick="download()">保存</button>

</div>

<canvas id="canvas" width="700" height="450"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

ctx.fillStyle = "#000"; ctx.fillRect(0,0,canvas.width,canvas.height);



let particles = [];



canvas.addEventListener("click", e => {

  let rect = canvas.getBoundingClientRect();

  let mx = e.clientX - rect.left;

  let my = e.clientY - rect.top;

  spray(mx, my);

});



function spray(x, y) {

  let count = +document.getElementById("count").value;

  let spread = +document.getElementById("spread").value;

  let psize = +document.getElementById("psize").value;

  let color = document.getElementById("color").value;



  for (let i = 0; i < count; i++) {

    let angle = Math.random() * Math.PI * 2;

    let dist = Math.random() * spread;

    let px = x + Math.cos(angle) * dist;

    let py = y + Math.sin(angle) * dist;



    particles.push({

      x: px, y: py,

      size: psize * (0.5 + Math.random()),

      color: color,

      life: 1,

      vx: (Math.random() - 0.5),

      vy: (Math.random() - 0.5) * 0.5 - 0.2

    });

  }

  drawParticles();

}



function drawParticles() {

  // 不清空画布，粒子叠加

  particles.forEach(p => {

    ctx.beginPath();

    ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);

    ctx.fillStyle = p.color;

    ctx.globalAlpha = p.life;

    ctx.fill();

    ctx.globalAlpha = 1;

  });



  // 动画漂移粒子（关闭旧粒子）

  particles = particles.filter(p => {

    p.x += p.vx;

    p.y += p.vy;

    p.life -= 0.02;

    return p.life > 0;

  });



  if (particles.length > 0) {

    requestAnimationFrame(drawParticles);

    // 重绘背景（可选：让粒子渐渐消失）

  }

}



function clearCanvas() {

  ctx.fillStyle = "#000"; ctx.fillRect(0,0,canvas.width,canvas.height);

  particles = [];

}

function download() {

  let a = document.createElement("a");

  a.download = "particle_art.png"; a.href = canvas.toDataURL(); a.click();

}

</script></body></html>

```

