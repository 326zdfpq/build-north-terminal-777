# 拖动喂食小游戏



## 说明

拖动食物喂给小动物，每喂一个不同的食物会增加分数。训练小朋友的手眼协调。



## 代码

```html

<!DOCTYPE html>

<html lang="zh">

<head><meta charset="UTF-8"><title>喂食游戏</title>

<style>

body{font-family:Arial;text-align:center;background:linear-gradient(#a8e6cf,#d4a5a5);min-height:100vh;margin:0;padding:20px}

.pet{font-size:80px;margin:30px;animation: bounce 1s infinite alternate}

@keyframes bounce{from{transform:translateY(0)}to{transform:translateY(-15px)}}

.food-bar{display:flex;justify-content:center;gap:20px;margin:30px}

.food{font-size:50px;cursor:grab;padding:10px;border-radius:12px;background:rgba(255,255,255,0.8);user-select:none;transition:transform 0.2s}

.food:hover{transform:scale(1.2)}

.mouth{width:120px;height:60px;border-radius:50%;border:3px dashed #333;display:flex;align-items:center;justify-content:center;font-size:14px;position:absolute;left:50%;transform:translateX(-50%);margin-top:-10px}

.mouth.over{border-color:#ff6b6b;background:rgba(255,0,0,0.1)}

#score{font-size:24px;color:#333}

#happiness{font-size:30px}

</style></head>

<body>

<h2>🐱 喂食小猫咪</h2>

<div id="happiness">😊</div>

<div id="score">分数: 0</div>

<div style="position:relative;display:inline-block">

  <div class="pet" id="pet">🐱</div>

  <div class="mouth" id="mouth">🍴 放这里</div>

</div>

<div class="food-bar" id="foodBar"></div>



<script>

const foods = ["🐟","🥩","🥛","🍗","🥚","🧀"];

let score = 0;

let fed = [];



function render() {

  let bar = document.getElementById("foodBar");

  bar.innerHTML = "";

  foods.forEach((f,i) => {

    let div = document.createElement("div");

    div.className = "food"; div.textContent = f;

    div.draggable = true;

    div.addEventListener("dragstart", e => e.dataTransfer.setData("text", JSON.stringify({food:f, idx:i})));

    bar.appendChild(div);

  });

}



let mouth = document.getElementById("mouth");

mouth.addEventListener("dragover", e => { e.preventDefault(); mouth.classList.add("over"); });

mouth.addEventListener("dragleave", () => mouth.classList.remove("over"));

mouth.addEventListener("drop", e => {

  e.preventDefault(); mouth.classList.remove("over");

  let data = JSON.parse(e.dataTransfer.getData("text"));

  if (fed.includes(data.food)) {

    document.getElementById("happiness").textContent = "😾 这个吃过了！";

    setTimeout(() => document.getElementById("happiness").textContent = "😊", 1500);

  } else {

    fed.push(data.food);

    score += 10;

    document.getElementById("score").textContent = "分数: " + score;

    document.getElementById("happiness").textContent = "😻";

    document.getElementById("pet").style.animation = "none";

    setTimeout(() => {

      document.getElementById("pet").style.animation = "bounce 1s infinite alternate";

      document.getElementById("happiness").textContent = "😊";

    }, 1000);

  }

});



render();

</script></body></html>

```

