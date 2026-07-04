# 圣诞树装饰画板



## 说明

在圣诞树上点击来挂装饰球。用户可以用装饰球装饰圣诞树。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>装饰圣诞树</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#0d3b0d;color:white}

canvas{background:linear-gradient(#1a237e,#0d47a1);border-radius:10px}

.tools{display:flex;justify-content:center;gap:8px;margin:10px;flex-wrap:wrap}

.color-btn{width:25px;height:25px;border-radius:50%;border:2px solid white;cursor:pointer}

</style></head>

<body>

<h2>🎄 装饰圣诞树 - 点击放装饰球</h2>

<div class="tools">

  <span>装饰颜色:</span>

  <div style="display:flex;gap:5px" id="colors"></div>

  <button onclick="resetTree()" style="background:#c62828;color:white;border:none;padding:8px 20px;border-radius:8px;cursor:pointer">重画</button>

</div>

<canvas id="canvas" width="500" height="500"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

let ornaments = [];

let currentColor = "#ff0000";



function drawTree() {

  ctx.clearRect(0,0,500,500);

  // 树干

  ctx.fillStyle = "#5d4037"; ctx.fillRect(220,350,60,100);

  // 树叶（三重三角形）

  ctx.fillStyle = "#2e7d32";

  ctx.beginPath(); ctx.moveTo(250,50); ctx.lineTo(100,200); ctx.lineTo(400,200); ctx.fill();

  ctx.fillStyle = "#388e3c";

  ctx.beginPath(); ctx.moveTo(250,100); ctx.lineTo(120,270); ctx.lineTo(380,270); ctx.fill();

  ctx.fillStyle = "#43a047";

  ctx.beginPath(); ctx.moveTo(250,160); ctx.lineTo(140,340); ctx.lineTo(360,340); ctx.fill();

  // 星星

  ctx.fillStyle = "#ffd700";

  ctx.font = "40px Arial"; ctx.fillText("⭐",225,65);

}



canvas.addEventListener("click", e => {

  let rect = canvas.getBoundingClientRect();

  let x = e.clientX - rect.left, y = e.clientY - rect.top;

  ornaments.push({x, y, color: currentColor});

  drawAll();

});



function drawAll() {

  drawTree();

  ornaments.forEach(o => {

    ctx.beginPath(); ctx.arc(o.x, o.y, 8, 0, Math.PI*2);

    ctx.fillStyle = o.color; ctx.fill();

    ctx.strokeStyle = "rgba(255,255,255,0.5)"; ctx.lineWidth = 1;

    ctx.stroke();

    // 高光

    ctx.beginPath(); ctx.arc(o.x-2, o.y-2, 3, 0, Math.PI*2);

    ctx.fillStyle = "rgba(255,255,255,0.6)"; ctx.fill();

  });

}



function resetTree() { ornaments = []; drawAll(); }



["#ff0000","#ffd700","#4fc3f7","#ce93d8","#ff8a65","#ffffff","#aed581","#ff80ab"].forEach((c,i) => {

  let btn = document.createElement("div"); btn.className="color-btn"; btn.style.background=c;

  btn.addEventListener("click",()=>{currentColor=c;document.querySelectorAll(".color-btn").forEach(b=>b.style.borderColor="white");btn.style.borderColor="#ff0";});

  document.getElementById("colors").appendChild(btn);

});



drawTree();

</script></body></html>

```

