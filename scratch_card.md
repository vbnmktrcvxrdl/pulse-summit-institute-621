# 刮刮卡揭晓



## 说明

模拟刮刮卡效果，手指滑动涂抹灰色涂层，露出下方隐藏文字。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1">

<title>刮刮卡</title>

<style>

body{text-align:center;font-family:Arial;margin:30px;background:#f5f5f5}

.card{width:300px;height:180px;margin:20px auto;position:relative;border-radius:12px;overflow:hidden;box-shadow:0 4px 20px rgba(0,0,0,0.15);user-select:none}

.prize{width:100%;height:100%;display:flex;align-items:center;justify-content:center;flex-direction:column;background:linear-gradient(135deg,#ffd700,#ffa000);font-size:36px;font-weight:bold;color:#5d4037}

.coat{position:absolute;top:0;left:0;right:0;bottom:0;background:#9e9e9e;cursor:grab}

.coat::after{content:'刮开有奖✨';position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);color:#fff;font-size:20px}

canvas{position:absolute;top:0;left:0}

#newBtn{display:none;margin:15px;padding:10px 30px;background:#4CAF50;color:white;border:none;border-radius:8px;font-size:18px;cursor:pointer}

</style></head>

<body>

<h2>🎁 刮刮卡</h2>

<div class="card" id="card">

  <div class="prize" id="prize">🎁 一等奖!</div>

  <canvas id="scratch" class="coat"></canvas>

</div>

<button id="newBtn" onclick="newCard()">再来一张</button>



<script>

const prizes = ["🎉 一等奖!", "💰 二等奖!", "🍬 三等奖!", "😅 谢谢参与", "⭐ 幸运奖!", "🎁 特等奖!"];

const canvas = document.getElementById("scratch");

const ctx = canvas.getContext("2d");

let scratching = false;

let scratched = 0;



function initScratch() {

  canvas.width = canvas.parentElement.clientWidth;

  canvas.height = canvas.parentElement.clientHeight;

  ctx.fillStyle = "#9e9e9e";

  ctx.fillRect(0, 0, canvas.width, canvas.height);

  ctx.font = "20px Arial";

  ctx.fillStyle = "white";

  ctx.textAlign = "center";

  ctx.fillText("刮开有奖✨", canvas.width/2, canvas.height/2);

  scratched = 0;

}



function getPos(e) {

  let rect = canvas.getBoundingClientRect();

  return { x: e.clientX - rect.left, y: e.clientY - rect.top };

}



function scratch(e) {

  let p = e.touches ? getPos(e.touches[0]) : getPos(e);

  ctx.globalCompositeOperation = "destination-out";

  ctx.beginPath();

  ctx.arc(p.x, p.y, 25, 0, Math.PI*2);

  ctx.fill();

  scratched++;



  // 刮开 40% 自动揭开

  let total = canvas.width * canvas.height / (Math.PI * 25 * 25);

  if (scratched / total > 0.4) {

    document.getElementById("card").querySelector(".coat").style.display = "none";

    document.getElementById("newBtn").style.display = "inline-block";

  }

}



canvas.addEventListener("mousedown", e => { scratching = true; scratch(e); });

canvas.addEventListener("mousemove", e => { if(scratching) scratch(e); });

canvas.addEventListener("mouseup", () => scratching = false);

canvas.addEventListener("touchstart", e => { e.preventDefault(); scratching = true; scratch(e); });

canvas.addEventListener("touchmove", e => { e.preventDefault(); if(scratching) scratch(e); });

canvas.addEventListener("touchend", () => scratching = false);



function newCard() {

  document.getElementById("prize").innerHTML = prizes[Math.floor(Math.random()*prizes.length)];

  document.getElementById("card").querySelector(".coat").style.display = "block";

  document.getElementById("newBtn").style.display = "none";

  initScratch();

}



initScratch();

</script></body></html>

```

