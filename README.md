# Araba-racing<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Basit Mobil Araba Oyunu</title>
<style>
  body { margin:0; overflow:hidden; background:#222; }
  #car {
    width:50px;
    height:100px;
    background:red;
    position:absolute;
    bottom:20px;
    left:50%;
    transform:translateX(-50%);
    border-radius:8px;
  }
  .obstacle {
    width:50px;
    height:50px;
    background:yellow;
    position:absolute;
  }
</style>
</head>
<body>

<div id="car"></div>

<script>
let car = document.getElementById("car");
let carX = window.innerWidth / 2 - 25;
let speed = 4;
let obstacles = [];

document.addEventListener("touchstart", (e)=>{
    let x = e.touches[0].clientX;
    if (x < window.innerWidth/2) carX -= 40;
    else carX += 40;
});

function spawnObstacle() {
    let obs = document.createElement("div");
    obs.className = "obstacle";
    obs.style.left = Math.random() * (window.innerWidth - 50) + "px";
    obs.style.top = "-60px";
    document.body.appendChild(obs);
    obstacles.push(obs);
}

setInterval(spawnObstacle, 1200);

function loop() {
    car.style.left = carX + "px";

    // Engelleri hareket ettir
    for (let obs of obstacles) {
        let y = parseFloat(obs.style.top);
        obs.style.top = (y + speed) + "px";

        // Çarpışma kontrol
        let carRect = car.getBoundingClientRect();
        let obsRect = obs.getBoundingClientRect();

        if (!(carRect.right < obsRect.left ||
              carRect.left > obsRect.right ||
              carRect.bottom < obsRect.top ||
              carRect.top > obsRect.bottom)) {
            alert("ÇARPTIN! Oyun yeniden başlayacak.");
            location.reload();
        }
    }

    requestAnimationFrame(loop);
}
loop();
</script>

</body>
</html>
