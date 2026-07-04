# 填充游戏



## 说明

在封闭形状内点击填充颜色，实现 Flood Fill 算法。图形学中最经典的算法之一。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>填充游戏</title>

<style>

body{text-align:center;font-family:Arial;margin:10px}

canvas{border:2px solid #333;cursor:pointer}

.tools{margin:10px}

.color-btn{display:inline-block;width:25px;height:25px;border:2px solid #333;border-radius:50%;cursor:pointer;margin:3px}

.color-btn.active{box-shadow:0 0 5px blue}

</style></head>

<body>

<h2>🎨 填充游戏 - 点击封闭区域填色</h2>

<div class="tools" id="colors"></div>

<canvas id="canvas" width="500" height="500"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");



// 画初始图案

ctx.fillStyle = "#fff"; ctx.fillRect(0,0,500,500);

ctx.strokeStyle = "#000"; ctx.lineWidth = 3;

ctx.strokeRect(20,20,150,150);

ctx.beginPath(); ctx.arc(300,100,60,0,Math.PI*2); ctx.stroke();

ctx.strokeRect(200,200,100,100);

ctx.strokeRect(350,220,120,120);



const palette = ["#ff0000","#ff9800","#ffeb3b","#4caf50","#2196f3","#9c27b0"];

let fillColor = palette[0];



// 颜色选择

let palDiv = document.getElementById("colors");

palette.forEach(c => {

  let btn = document.createElement("div");

  btn.className = "color-btn"; btn.style.background = c;

  btn.addEventListener("click", () => {

    fillColor = c; document.querySelectorAll(".color-btn").forEach(b => b.classList.remove("active"));

    btn.classList.add("active");

  });

  if (c === fillColor) btn.classList.add("active");

  palDiv.appendChild(btn);

});



// Flood Fill 算法

canvas.addEventListener("click", e => {

  let rect = canvas.getBoundingClientRect();

  let x = Math.floor(e.clientX - rect.left);

  let y = Math.floor(e.clientY - rect.top);



  let imgData = ctx.getImageData(0, 0, canvas.width, canvas.height);

  let data = imgData.data;

  let w = canvas.width;



  let targetIdx = (y * w + x) * 4;

  let r0 = data[targetIdx], g0 = data[targetIdx+1], b0 = data[targetIdx+2];



  // 颜色匹配函数（考虑微小误差）

  function match(ri, gi, bi) {

    return Math.abs(ri-r0)<5 && Math.abs(gi-g0)<5 && Math.abs(bi-b0)<5;

  }



  // 解析填充色

  let hex = fillColor.replace("#", "");

  let fr = parseInt(hex.substring(0,2),16);

  let fg = parseInt(hex.substring(2,4),16);

  let fb = parseInt(hex.substring(4,6),16);



  // BFS 填充

  let queue = [[x, y]];

  let visited = new Set();

  let h = canvas.height;



  while (queue.length > 0) {

    let [cx, cy] = queue.shift();

    let key = cx + "," + cy;

    if (visited.has(key)) continue;

    if (cx < 0 || cy < 0 || cx >= w || cy >= h) continue;



    let idx = (cy * w + cx) * 4;

    if (!match(data[idx], data[idx+1], data[idx+2])) continue;



    visited.add(key);

    data[idx] = fr; data[idx+1] = fg; data[idx+2] = fb; data[idx+3] = 255;



    queue.push([cx+1, cy], [cx-1, cy], [cx, cy+1], [cx, cy-1]);

  }



  ctx.putImageData(imgData, 0, 0);

});

</script></body></html>

```

