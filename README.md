<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Гордей выживает</title>
<style>
  body {
    margin: 0;
    background: #111;
    color: white;
    font-family: monospace;
    text-align: center;
  }
  canvas {
    background: #4aa34a;
    display: block;
    margin: 10px auto;
    border: 4px solid #000;
  }
</style>
</head>
<body>

<h1>🧱 Гордей, выживай нахуй</h1>
<p>WASD — ходить | Клик — ставить блок | Зомби Марк хочет тебя сожрать</p>

<canvas id="game" width="640" height="480"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const TILE = 32;
const mapW = 20;
const mapH = 15;

let world = Array(mapH).fill().map(()=>Array(mapW).fill(0));

let gordey = { x: 5, y: 5, hp: 100 };
let zombies = [{ x: 12, y: 8, name: "Марк", hp: 50 }];

document.addEventListener("keydown", e => {
  if (e.key === "w") gordey.y--;
  if (e.key === "s") gordey.y++;
  if (e.key === "a") gordey.x--;
  if (e.key === "d") gordey.x++;
});

canvas.addEventListener("click", e => {
  const rect = canvas.getBoundingClientRect();
  const x = Math.floor((e.clientX - rect.left) / TILE);
  const y = Math.floor((e.clientY - rect.top) / TILE);
  world[y][x] = 1;
});

function update() {
  zombies.forEach(z => {
    if (Math.random() < 0.02) {
      z.x += Math.sign(gordey.x - z.x);
      z.y += Math.sign(gordey.y - z.y);
    }
    if (z.x === gordey.x && z.y === gordey.y) {
      gordey.hp -= 1;
      if (gordey.hp <= 0) {
        alert("Гордей сдох. Марк победил, пиздец.");
        location.reload();
      }
    }
  });
}

function draw() {
  ctx.clearRect(0,0,640,480);

  for (let y=0;y<mapH;y++) {
    for (let x=0;x<mapW;x++) {
      if (world[y][x]) {
        ctx.fillStyle = "#8b5a2b";
        ctx.fillRect(x*TILE,y*TILE,TILE,TILE);
      }
    }
  }

  ctx.fillStyle = "blue";
  ctx.fillRect(gordey.x*TILE, gordey.y*TILE, TILE, TILE);
  ctx.fillText("Гордей", gordey.x*TILE, gordey.y*TILE-2);

  zombies.forEach(z => {
    ctx.fillStyle = "green";
    ctx.fillRect(z.x*TILE, z.y*TILE, TILE, TILE);
    ctx.fillText(z.name, z.x*TILE, z.y*TILE-2);
  });

  ctx.fillStyle = "white";
  ctx.fillText("HP Гордея: " + gordey.hp, 10, 15);
}

function loop() {
  update();
  draw();
  requestAnimationFrame(loop);
}

loop();
</script>

</body>
</html>
