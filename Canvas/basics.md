```js
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");

// //Drawing Shapes
// ctx.fillStyle = "blue";
// ctx.fillRect(50, 50, 100, 100);

// //Circle

// ctx.beginPath();
// ctx.arc(200, 200, 50, 0, Math.PI * 2); // x, y, radius, start angle, end angle
// ctx.fill();

// //Drawing Text
// ctx.fillStyle = "red";
// ctx.font = "30px Arial";
// ctx.fillText("Hello Canvas", 100, 100);

//Line
// ctx.fillStyle = "red";
// ctx.moveTo(0, 0);
// ctx.lineTo(200, 100);
// ctx.stroke();

//Stroke text
// ctx.font = "30px Arial";
// ctx.strokeText("Hello World", 10, 50);

// Create gradient
var grd = ctx.createLinearGradient(0, 0, 200, 0);
grd.addColorStop(0, "red");
grd.addColorStop(1, "white");

// Fill with gradient
ctx.fillStyle = grd;
ctx.fillRect(10, 10, 150, 80);

```

# Advance Version
![[Pasted image 20241026113037.png]]
