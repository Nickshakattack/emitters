Emitters homework; New Updated:

let emitters = [];
let G;

function setup() {
  createCanvas(400, 600);
  emitters.push(new Emitter(width / 2, 30));
  G = createVector(0, 0.1);
  ellipseMode(RADIUS);
  noStroke();
}

function draw() {
  background(220);
  for (let e of emitters) {
    e.update();
  }
}

function mouseClicked() {
  emitters.push(new Emitter(mouseX, mouseY));
}

class Emitter {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.particles = [];
    this.timer = 0;

    for (let i = 0; i < 20; i++) {
      this.particles.push(this.createRandomParticle());
    }
  }

  createRandomParticle() {
    let type = random([0, 1, 2]);
    if (type === 0) {
      return new Particle(this.x, this.y);
    } else if (type === 1) {
      return new ColorParticle(this.x, this.y);
    } else {
      return new SquareParticle(this.x, this.y);
    }
  }

  update() {
    this.particles = this.particles.filter(p => !p.isDead());

    for (let p of this.particles) {
      p.applyForce(G);
      p.update();
      p.draw();
    }

    this.timer++;
    if (this.timer % 4 === 0) {
      this.particles.push(this.createRandomParticle());
    }
  }
}

class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(random(-1, 1), random(-1, 1));
    this.acc = createVector(0, 0);
    this.lifetime = 255;
    this.r = random(3, 7);
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifetime -= 2;
  }

  draw() {
    fill(100);
    ellipse(this.pos.x, this.pos.y, this.r);
  }

  isDead() {
    return this.lifetime <= 0;
  }
}

class ColorParticle extends Particle {
  constructor(x, y) {
    super(x, y);
    this.color = color(random(255), random(255), random(255));
  }

  draw() {
    fill(this.color);
    ellipse(this.pos.x, this.pos.y, this.r);
  }
}

class SquareParticle extends Particle {
  constructor(x, y) {
    super(x, y);
  }

  draw() {
    fill(50, 100, 200);
    rectMode(CENTER);
    rect(this.pos.x, this.pos.y, this.r * 2, this.r * 2);
  }
}


  isDead() {
    return this.lifetime <= 0;
  }
}

