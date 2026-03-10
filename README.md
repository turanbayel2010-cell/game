canvas id="game" width="400" height="400"></canvas>
<div>Score: <span id="score">0</span></div>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const box = 20;
let snake = [{x: 200, y: 200}];
let food = {
  x: Math.floor(Math.random()*20)*box,
  y: Math.floor(Math.random()*20)*box
};

let dx = box;
let dy = 0;
let score = 0;

document.addEventListener("keydown", direction);

function direction(e){
  if(e.key === "ArrowLeft" && dx === 0){ dx = -box; dy = 0; }
  else if(e.key === "ArrowUp" && dy === 0){ dx = 0; dy = -box; }
  else if(e.key === "ArrowRight" && dx === 0){ dx = box; dy = 0; }
  else if(e.key === "ArrowDown" && dy === 0){ dx = 0; dy = box; }
}

function collision(head, arr){
  for(let i=0;i<arr.length;i++){
    if(head.x === arr[i].x && head.y === arr[i].y){
      return true;
    }
  }
  return false;
}

function draw(){
  ctx.fillStyle = "black";
  ctx.fillRect(0,0,400,400);

  for(let i=0;i<snake.length;i++){
    ctx.fillStyle = i===0 ? "lime" : "green";
    ctx.fillRect(snake[i].x, snake[i].y, box, box);
  }

  ctx.fillStyle = "red";
  ctx.fillRect(food.x, food.y, box, box);

  let headX = snake[0].x + dx;
  let headY = snake[0].y + dy;

  if(headX === food.x && headY === food.y){
    score++;
    document.getElementById("score").textContent = score;
    food = {
      x: Math.floor(Math.random()*20)*box,
      y: Math.floor(Math.random()*20)*box
    };
  } else {
    snake.pop();
  }

  const newHead = {x: headX, y: headY};

  if(
    headX < 0 || headY < 0 ||
    headX >= 400 || headY >= 400 ||
    collision(newHead, snake)
  ){
    clearInterval(game);
    alert("Game Over. Score: " + score);
  }

  snake.unshift(newHead);
}

let game = setInterval(draw, 100);
</script>
