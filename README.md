# agrinho-2025
Repositório destinado ao projeto agrínho 2025
projeto desenvolvido com IA GEMINI, prompt:
let balloons = []; // Array para armazenar os balões
let score = 0;
let backgroundImg;
let balloonImg; // Para carregar uma imagem de balão customizada (opcional)

function preload() {
  // Você pode criar ou encontrar imagens para deixar o jogo mais bonito!
  // Por exemplo, um fundo de céu estrelado e balões juninos.
  // backgroundImg = loadImage('sky_background.png');
  // balloonImg = loadImage('balloon.png');
}

function setup() {
  createCanvas(800, 600);
  // Se você tiver uma imagem de fundo, use-a aqui.
  // background(backgroundImg);
  spawnBalloon(); // Cria o primeiro balão
}

function draw() {
  background(100, 150, 200); // Fundo azul escuro (céu) - ajuste para mais bonito!

  // Desenha algumas estrelas (exemplo simples)
  fill(255, 255, 150, 200); // Amarelo claro e semi-transparente
  for (let i = 0; i < 20; i++) {
    ellipse(random(width), random(height / 2), 2, 2);
  }

  // Desenha os balões
  for (let i = balloons.length - 1; i >= 0; i--) {
    let b = balloons[i];
    b.display();
    b.move();

    // Remove balões que saem da tela
    if (b.offscreen()) {
      balloons.splice(i, 1);
      // Se você quiser que o jogador perca pontos ou vidas ao deixar balões passarem, adicione aqui.
    }
  }

  // Gera novos balões periodicamente
  if (frameCount % 60 === 0) { // Cria um novo balão a cada segundo (60 frames)
    spawnBalloon();
  }

  // Exibe a pontuação
  fill(255);
  textSize(24);
  textAlign(LEFT, TOP);
  text('Pontos: ' + score, 10, 10);
}

function mousePressed() {
  for (let i = balloons.length - 1; i >= 0; i--) {
    let b = balloons[i];
    if (b.contains(mouseX, mouseY)) {
      score += 10; // Adiciona pontos ao estourar
      balloons.splice(i, 1); // Remove o balão estourado
      // Adicionar um som ou animação de estouro aqui seria legal!
    }
  }
}

function spawnBalloon() {
  let x = random(width);
  let y = height + 50; // Começa um pouco abaixo da tela
  let r = random(20, 40); // Tamanho aleatório do balão
  let speed = random(1, 3); // Velocidade aleatória
  let color = color(random(150, 255), random(150, 255), random(150, 255)); // Cor aleatória
  balloons.push(new Balloon(x, y, r, speed, color));
}

// Classe Balão
class Balloon {
  constructor(x, y, r, speed, color) {
    this.x = x;
    this.y = y;
    this.r = r; // Raio do balão
    this.speed = speed;
    this.color = color;
  }

  display() {
    noStroke();
    fill(this.color);
    ellipse(this.x, this.y, this.r * 2); // Desenha o corpo do balão

    // Adiciona um pequeno "bico" do balão
    triangle(this.x, this.y + this.r, this.x - 5, this.y + this.r + 15, this.x + 5, this.y + this.r + 15);

    // Se tiver uma imagem de balão, use-a no lugar do ellipse e triangle:
    // if (balloonImg) {
    //   image(balloonImg, this.x - this.r, this.y - this.r, this.r * 2, this.r * 2);
    // }
  }

  move() {
    this.y -= this.speed;
  }

  offscreen() {
    return this.y < -this.r; // Balão saiu da parte superior da tela
  }

  contains(px, py) {
    let d = dist(px, py, this.x, this.y);
    return d < this.r; // Verifica se o clique está dentro do balão
  }
}
