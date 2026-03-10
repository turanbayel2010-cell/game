<!DOCTYPE html>
<html>
<body>

<canvas id="game" width="400" height="400"></canvas>
<div>Score: <span id="score">0</span></div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const size = 20;
let snake = [{x:10, y:10}];
let food = {x:5, y:5};

let dx = 1;
let dy = 0;
let score = 0;

document.addEventListener("keydown", e=>{
 if(e.key==="ArrowUp" && dy===0){dx=0;dy=-1;}
 if(e.key==="ArrowDown" && dy===0){dx=0;dy=1;}
 if(e.key==="ArrowLeft" && dx===0){dx=-1;dy=0;}
 if(e.key==="ArrowRight" && dx===0){dx=1;dy=0;}
});

function draw(){

 ctx.fillStyle="black";
 ctx.fillRect(0,0,400,400);

 snake.forEach((s,i)=>{
  ctx.fillStyle=i?"green":"lime";
  ctx.fillRect(s.x*size,s.y*size,size,size);
 });

 ctx.fillStyle="red";
 ctx.fillRect(food.x*size,food.y*size,size,size);

 let head={
  x:snake[0].x+dx,
  y:snake[0].y+dy
 };

 if(head.x===food.x && head.y===food.y){
  score++;
  document.getElementById("score").textContent=score;

  food={
   x:Math.floor(Math.random()*20),
   y:Math.floor(Math.random()*20)
  };
 }else{
  snake.pop();
 }

 if(
  head.x<0 || head.y<0 ||
  head.x>=20 || head.y>=20 ||
  snake.some(s=>s.x===head.x && s.y===head.y)
 ){
  alert("Game Over. Score: "+score);
  location.reload();
 }

 snake.unshift(head);
}

setInterval(draw,100);
</script>

</body>
</html>
