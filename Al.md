# Yapay Zeka Araçları Kullanımı

Bu belge, projenin geliştirilmesi sırasında yararlanılan yapay zeka araçlarını ve bu araçlarla gerçekleştirilen etkileşimleri detaylandırmaktadır.

## ChatGPT

### Kullanılan Promptlar ve Cevaplar

---

**Prompt 1:**

Chat, HTML5 ve JavaScript ile Canvas kullanarak basit bir şekilde oyun tasarlamak istiyorum. Oyunumun temel mekaniği bazı oyuncakların hareket ettirilip duvardaki gölgeleriyle duvarda bazı şekiller oluşturmak ve Dream yazılı bir butona tıklandığında ekranın değişip amacı karakterin, engel olarak görünen bazı resimlere değmemeye çalışarak bu engelleri aşarak kale resmine ulaşmak olduğu bir oyun. Giriş sayfası, gerçek dünya sayfası ve hayal dünyası sayfası olacak bunlar birbirlerine butonlarla bağlanacak. Start-Dream-Reality şeklinde. Nasıl yapabilirim?

**Cevap 1:**

Bu çok yaratıcı ve güzel bir fikir! Bu oyunu temel adımlarla nasıl kurabileceğini aşağıda açıklıyorum. Oyun üç ana sahneye sahip olacak: Giriş (Start), Gerçek Dünya (Reality) ve Gölge Dünyası (Dream). İşte genel yapısı:

1. **HTML Yapısı**

```html
<canvas id="gameCanvas" width="800" height="600"></canvas>
<button id="startBtn">Start</button>
<button id="dreamBtn" style="display:none;">Dream</button>
<button id="realityBtn" style="display:none;">Reality</button>
```

2. **CSS (İsteğe Bağlı)**

```css
canvas {
  display: block;
  margin: 0 auto;
  background: #e0f7fa;
}
button {
  display: block;
  margin: 10px auto;
  padding: 10px 20px;
}
```

3. **JavaScript – Temel Mantık**

```javascript
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let currentScene = 'start'; // start | reality | dream

document.getElementById('startBtn').onclick = () => {
  currentScene = 'reality';
  document.getElementById('startBtn').style.display = 'none';
  document.getElementById('dreamBtn').style.display = 'block';
  drawScene();
};

document.getElementById('dreamBtn').onclick = () => {
  currentScene = 'dream';
  document.getElementById('dreamBtn').style.display = 'none';
  document.getElementById('realityBtn').style.display = 'block';
  drawScene();
};

document.getElementById('realityBtn').onclick = () => {
  currentScene = 'reality';
  document.getElementById('realityBtn').style.display = 'none';
  document.getElementById('dreamBtn').style.display = 'block';
  drawScene();
};

function drawScene() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  if (currentScene === 'start') {
    drawStart();
  } else if (currentScene === 'reality') {
    drawReality();
  } else if (currentScene === 'dream') {
    drawDream();
  }
}

function drawStart() {
  ctx.font = '30px Arial';
  ctx.fillStyle = 'black';
  ctx.fillText('Welcome to the Shadow Puzzle Game!', 100, 300);
}

function drawReality() {
  ctx.fillStyle = 'skyblue';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  // Burada oyuncaklar, ışık, gölge ve duvar çizimi yapılacak
}

function drawDream() {
  ctx.fillStyle = 'darkblue';
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  // Önceki sahnede oluşturulan gölge şekilleri burada engel olacak
}
```

4. **Yapılacaklar:**

* Gerçek dünyada oyuncakları sürükleyerek duvardaki gölge şekillerini oluşturma
* Bu gölge şekillerini dream sahnesine taşıyıp engellere dönüştürme
* Karakterin hareket etmesi ve hedefe ulaşması
* Gölge oluşturulmazsa Dream butonunun pasif olması

---

**Prompt 2:**

İlk olarak üç adet küpün ve oyuncak asker figürünün gölgelerini duvarda görmek istiyorum. Bu gölgeler duvara yaklaştıkça küçülsün duvardan uzaklaştıkça büyüsün.

**Cevap 2:**

Harika! Gölgenin duvarda görünmesini ve duvara yakınsa küçük, uzaksa büyük olmasını istiyorsun. Bu etkiyi yaratmak için:

```javascript
function drawShadowOnWall(obj, isSoldier = false) {
  const objCenterX = obj.x + (isSoldier ? obj.width / 2 : obj.size / 2);
  const objCenterY = obj.y + (isSoldier ? obj.height / 2 : obj.size / 2);
  const distance = Math.max(objCenterY - duvarHeight, 50);
  const scale = Math.min(distance / 300, 1.5);
  const shadowWidth = isSoldier ? 40 * scale : 50 * scale;
  const shadowHeight = isSoldier ? 60 * scale : 40 * scale;

  ctx.fillStyle = "rgba(80, 80, 80, 0.3)";
  ctx.beginPath();
  ctx.ellipse(objCenterX, duvarHeight - 10, shadowWidth, shadowHeight, 0, 0, Math.PI * 2);
  ctx.fill();
}
```

Ve sahne çiziminde:

```javascript
// DUVAR GÖLGELERİ
for (let cube of cubes) drawShadowOnWall(cube);
drawShadowOnWall(soldier, true);
```

---

**Prompt 3:**

Şimdi hayal dünyasına 3 adet dikdörtgen resmi eklemek istiyorum. Bunlar saridd.png, kirmizidd.png ve mavidd.png resimleri olacak. Yazdığım sırayla ekle ve alt çizgileri ekranın alt çizgisine yakın ve boyları da giderek uzasın yani basamaklı bir yapı istiyorum. Bu kodları ve nereye eklemem gerektiğini söyler misin lütfen?

**Cevap 3:**

1. **Resimleri yükle:**

```javascript
const saridd = new Image();
saridd.src = "saridd.png";

const kirmizidd = new Image();
kirmizidd.src = "kirmizidd.png";

const mavidd = new Image();
mavidd.src = "mavidd.png";
```

2. **drawDreamWorld fonksiyonunda çiz:**

```javascript
function drawDreamWorld() {
  ctx.fillStyle = "#1a1a1a";
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  const baseY = canvas.height - 50;
  const rectWidth = 50;
  const spacing = 20;

  ctx.drawImage(saridd, 100, baseY - 100, rectWidth, 100);
  ctx.drawImage(kirmizidd, 100 + rectWidth + spacing, baseY - 150, rectWidth, 150);
  ctx.drawImage(mavidd, 100 + 2 * (rectWidth + spacing), baseY - 200, rectWidth, 200);
}
```

3. **draw fonksiyonunda çağır:**

```javascript
function draw() {
  if (gameState === "dream") {
    drawDreamWorld();
  }
  requestAnimationFrame(draw);
}
```
İstersen bu yapılara gölge efekti, ışık yansıması veya etkileşim de ekleyebiliriz. Yardımcı olmaktan memnuniyet duyarım.
