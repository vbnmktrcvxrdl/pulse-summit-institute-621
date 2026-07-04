# 随机涂色页面



## 说明

点击画布随机生成一个带颜色的几何图案。每次点击都有惊喜，适合减压。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>随机涂色</title>

<style>

body{text-align:center;font-family:Arial;margin:10px;background:#263238;color:white}

canvas{background:white;border:2px solid #37474f;cursor:pointer}

p{opacity:0.7}

</style></head>

<body>

<h2>🎲 随机几何涂色 - 点一下生成一个图案</h2>

<p>支持：圆、方形、三角、星形、六边形</p>

<canvas id="canvas" width="650" height="450"></canvas>



<script>

const canvas = document.getElementById("canvas");

const ctx = canvas.getContext("2d");

ctx.fillStyle = "#fff"; ctx.fillRect(0,0,canvas.width,canvas.height);



function randomColor() {

  return `hsl(${Math.random()*360}, ${60+Math.random()*40}%, ${50+Math.random()*30}%)`;

}



function drawRandomShape(x, y) {

  let size = 20 + Math.random() * 60;

  let type = Math.floor(Math.random() * 6);

  let color = randomColor();



  ctx.fillStyle = color;

  ctx.strokeStyle = "#333";

  ctx.lineWidth = 2;



  switch(type) {

    case 0: // 圆

      ctx.beginPath(); ctx.arc(x, y, size, 0, Math.PI*2); ctx.fill(); ctx.stroke();

      break;

    case 1: // 方形

      ctx.fillRect(x-size/2, y-size/2, size, size);

      ctx.strokeRect(x-size/2, y-size/2, size, size);

      break;

    case 2: // 三角

      ctx.beginPath(); ctx.moveTo(x, y-size);

      for(let i=0;i<3;i++) { let a=i*Math.PI*2/3; ctx.lineTo(x+Math.sin(a)*size, y+Math.cos(a)*size); }

      ctx.closePath(); ctx.fill(); ctx.stroke();

      break;

    case 3: // 五角星

      ctx.beginPath();

      for(let i=0;i<10;i++){

        let r = i%2===0 ? size : size*0.4;

        let a = (Math.PI/5)*i - Math.PI/2;

        i===0 ? ctx.moveTo(x+r*Math.cos(a),y+r*Math.sin(a)) : ctx.lineTo(x+r*Math.cos(a),y+r*Math.sin(a));

      }

      ctx.closePath(); ctx.fill(); ctx.stroke();

      break;

    case 4: // 六边形

      ctx.beginPath();

      for(let i=0;i<6;i++){ let a=(Math.PI/3)*i-Math.PI/6; i===0?ctx.moveTo(x+size*Math.cos(a),y+size*Math.sin(a)):ctx.lineTo(x+size*Math.cos(a),y+size*Math.sin(a));}

      ctx.closePath(); ctx.fill(); ctx.stroke();

      break;

    case 5: // 随机泼墨

      for(let i=0;i<30;i++){

        ctx.beginPath();ctx.arc(x+(Math.random()-0.5)*size*2,y+(Math.random()-0.5)*size*2,2+Math.random()*8,0,Math.PI*2);ctx.fill();

      }

      break;

  }

}



canvas.addEventListener("click", e => {

  let rect = canvas.getBoundingClientRect();

  drawRandomShape(e.clientX - rect.left, e.clientY - rect.top);

});

</script></body></html>

```

