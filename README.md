[clouds.html](https://github.com/user-attachments/files/21956200/clouds.html)[Uploading clou<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Interactive Clouds</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.min.js"></script>
</head>
<body>
<script>

let cloudAmount = 5, humidity = 60, windSpeed = 1.5, windDir = 0;
let temp = 15;       
let t = 0;

let currentColor, targetColor;
let intensity = 0;      
let maxIntensity = 10;  
let clickCount = 0;     

let clouds = [];

function setup() {
  createCanvas(800, 800);
  noStroke();
  frameRate(60);

  currentColor = color(255-50, 255-100, 255-200);
  targetColor = currentColor;

  for(let i=0; i<500; i++){
    clouds.push(new Cloud());
  }
}

function draw() {
  background(255);
  translate(width/2, height/2);

  // 온도 색상
  if (temp < 0) targetColor = color(255-80, 255-120, 255-200);
  else if (temp < 15) targetColor = color(255-100, 255-160, 255-255);
  else if (temp < 25) targetColor = color(255-180, 255-220, 255-140);
  else targetColor = color(255-255, 255-160, 255-100);

  currentColor = lerpColor(currentColor, targetColor, 0.04);

  for(let c of clouds){
    c.update();
    c.display();
  }

  t += windSpeed * 0.05 * intensity;

  let targetIntensity = 0.5;
  intensity += (targetIntensity - intensity) * 0.08;

  temp = lerp(temp, 15, 0.01);

  fill(0);
  textAlign(CENTER);
  textSize(24);
  text(clickCount, 0, height/2 - 20);
}

class Cloud {
  constructor(){
    this.baseRadius = random(0, 300);
    this.angle = random(TWO_PI);
    this.radius = this.baseRadius;
    this.size = map(this.baseRadius, 0, 300, 6, 1.5);
    this.alpha = map(this.baseRadius, 0, 300, 150, 50);
    this.lightning = false;
    this.lightningMaxSize = 0;
    this.updatePos();
  }

  updatePos(){
    this.x = cos(this.angle) * this.radius;
    this.y = sin(this.angle) * this.radius;
  }

  update(){
    let chaos = intensity * 0.5;
    this.angle += 0.002 + 0.01 * chaos;
    this.radius = this.baseRadius + sin(t*0.5 + this.angle*2) * 2 * chaos;
    this.updatePos();

    let lightningChance = intensity * map(clickCount, 1, 50, 0.2, 5);
    if(!this.lightning && random(1000) < lightningChance){
      this.lightning = true;
      this.lightningMaxSize = this.size*2 + map(clickCount, 1, 50, 2, 80);
    }

    if(this.lightning){
      let targetSize = this.size*2 + intensity*8;
      this.lightningMaxSize = lerp(this.lightningMaxSize, targetSize, 0.2);
      if(intensity < 0.6){
        this.lightning = false;
      }
    } else {
      this.lightningMaxSize = lerp(this.lightningMaxSize, 0, 0.2);
    }
  }

  display(){
    fill(0, this.alpha);
    ellipse(this.x, this.y, this.size, this.size);

    if(this.lightningMaxSize > 0.5){
      for(let i=0; i<3; i++){
        let factor = 0.5 + 0.25*i;
        fill(0, this.alpha * (1.0 - i*0.2));
        ellipse(this.x, this.y, this.lightningMaxSize*factor, this.lightningMaxSize*factor);
      }
    }
  }
}

function mousePressed() {
  clickCount++;
  let clickFactor = map(clickCount, 1, 50, 0.2, 5);
  intensity += clickFactor;
  intensity = constrain(intensity, 0.5, maxIntensity);

  cloudAmount += random(-0.5, 0.5);
  humidity += random(-3, 3);
  windSpeed += random(-0.2, 0.2);
  temp += random(-1.5, 1.5);

  cloudAmount = constrain(cloudAmount, 0, 10);
  humidity = constrain(humidity, 30, 100);
  windSpeed = constrain(windSpeed, 0.5, 3);
  temp = constrain(temp, -5, 35);
}

</script>
</body>
</html>
ds.html…]()
