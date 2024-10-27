```js
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");

const CANVAS_WIDTH = (canvas.width = 600);
const CANVAS_HEIGHT = (canvas.height = 600);
const SPRITE_WIDTH = 575; //total image width, no. of cols
const SPRITE_HEIGHT = 523; //whole image / rows
let frameX = 0;
let frameY = 0;
let gameFrame = 0; //for slow down
let maxFrame = 6;
const staggerFrames = 8;
//Bing Image
const playerImage = new Image();
playerImage.src = "./assets/shadow_dog.png";

function animate() {
  ctx.clearRect(0, 0, CANVAS_WIDTH, CANVAS_HEIGHT);
  //   ctx.fillRect(50, 50, 100, 100);
  //S -> Source -> Cut out part , D -> Dest=> Where to draw
  //ctx.drawImage(image,     sx, sy  sw, sh, dx,dy, dw, dh )
  ctx.drawImage(
    playerImage,
    frameX * SPRITE_WIDTH,
    frameY * SPRITE_HEIGHT,
    SPRITE_WIDTH,
    SPRITE_HEIGHT,
    0,
    0,
    SPRITE_WIDTH,
    SPRITE_HEIGHT
  );
  if (gameFrame % staggerFrames === 0) {
    if (frameX < maxFrame) frameX++;
    else frameX = 0;
  }
  gameFrame++;

  requestAnimationFrame(animate);
}
animate();

```