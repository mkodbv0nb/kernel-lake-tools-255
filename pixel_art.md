# 像素绘画



## 说明

点击网格像素格来绘制像素画，最终可导出为图片。类似像素风游戏的美术编辑器。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>像素绘画</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#2d2d2d;color:white}

#grid{display:grid;width:fit-content;margin:15px auto;border:2px solid #555}

.cell{width:20px;height:20px;border:1px solid #444;cursor:pointer}

.cell:hover{border-color:#fff}

.tools{margin:10px;display:flex;justify-content:center;gap:10px;flex-wrap:wrap}

.color-btn{width:30px;height:30px;border-radius:50%;border:2px solid white;cursor:pointer}

.color-btn.active{border-color:#ff0;box-shadow:0 0 8px #ff0}

button{padding:6px 16px;border:none;border-radius:6px;cursor:pointer}

</style></head>

<body>

<h2>🎮 像素绘画</h2>

<div class="tools">

  <span style="color:#ccc">颜色:</span>

  <div id="colors" style="display:flex;gap:5px"></div>

  <button onclick="fillAll()" style="background:#4CAF50;color:white">填充全部</button>

  <button onclick="clearAll()" style="background:#f44336;color:white">清空</button>

  <button onclick="exportImage()" style="background:#2196F3;color:white">导出图片</button>

</div>

<div id="grid"></div>

<canvas id="exportCanvas" style="display:none"></canvas>



<script>

const SIZE = 32;

const CELL = 20;

const colors = ["#ff0000","#ff8800","#ffff00","#00ff00","#00ffff","#0000ff","#ff00ff","#964B00","#000000","#ffffff"];



let grid = Array(SIZE).fill(null).map(() => Array(SIZE).fill(null));

let currentColor = colors[0];



// 颜色选择器

let colorDiv = document.getElementById("colors");

colors.forEach(c => {

  let btn = document.createElement("div");

  btn.className = "color-btn"; btn.style.background = c;

  btn.addEventListener("click", () => {

    currentColor = c;

    document.querySelectorAll(".color-btn").forEach(b => b.classList.remove("active"));

    btn.classList.add("active");

  });

  if (c === currentColor) btn.classList.add("active");

  colorDiv.appendChild(btn);

});



// 网格

let gridEl = document.getElementById("grid");

gridEl.style.gridTemplateColumns = `repeat(${SIZE}, ${CELL}px)`;

let drawing = false;



for (let r = 0; r < SIZE; r++) {

  for (let c = 0; c < SIZE; c++) {

    let cell = document.createElement("div");

    cell.className = "cell"; cell.style.background = grid[r][c] || "#1a1a1a";

    cell.dataset.r = r; cell.dataset.c = c;

    cell.addEventListener("mousedown", e => {

      drawing = true;

      grid[r][c] = currentColor;

      cell.style.background = currentColor;

      e.preventDefault();

    });

    cell.addEventListener("mouseenter", () => {

      if (drawing) { grid[r][c] = currentColor; cell.style.background = currentColor; }

    });

    cell.addEventListener("contextmenu", e => {

      e.preventDefault(); grid[r][c] = null; cell.style.background = "#1a1a1a";

    });

    gridEl.appendChild(cell);

  }

}

document.addEventListener("mouseup", () => drawing = false);



function fillAll() {

  for (let r = 0; r < SIZE; r++) for (let c = 0; c < SIZE; c++) {

    grid[r][c] = currentColor;

    document.querySelector(`[data-r="${r}"][data-c="${c}"]`).style.background = currentColor;

  }

}

function clearAll() {

  for (let r = 0; r < SIZE; r++) for (let c = 0; c < SIZE; c++) {

    grid[r][c] = null;

    document.querySelector(`[data-r="${r}"][data-c="${c}"]`).style.background = "#1a1a1a";

  }

}

function exportImage() {

  let cvs = document.getElementById("exportCanvas");

  cvs.width = SIZE * 4; cvs.height = SIZE * 4;

  let ctx = cvs.getContext("2d");

  for (let r = 0; r < SIZE; r++) for (let c = 0; c < SIZE; c++) {

    ctx.fillStyle = grid[r][c] || "#1a1a1a";

    ctx.fillRect(c*4, r*4, 4, 4);

  }

  let link = document.createElement("a");

  link.download = "pixel_art.png"; link.href = cvs.toDataURL(); link.click();

}

</script></body></html>

```

