<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Снеткод — Максим Снетков</title>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@500;900&family=Inter:wght@300;500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #0d0d0d;
      --accent: #00ffff;
      --text: #e0e0e0;
      --dark: #111;
    }
    * { margin:0; padding:0; box-sizing:border-box; }
    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Inter', sans-serif;
      overflow-x: hidden;
      line-height: 1.6;
      -webkit-tap-highlight-color: transparent; /* убирает серую подсветку при тапе на мобилках */
    }

    /* Глитч-эффект чуть поменьше на мобиле */
    .glitch {
      font-family: 'Orbitron', sans-serif;
      font-size: clamp(2.5rem, 12vw, 5rem);
      font-weight: 900;
      text-align: center;
      padding: 100px 20px 30px;
      background: linear-gradient(90deg, var(--accent), #ff00ff, var(--accent));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      position: relative;
      animation: glitch 6s infinite;
    }

    .subtitle {
      font-size: clamp(1.2rem, 4vw, 1.8rem);
      letter-spacing: 2px;
      opacity: 0.9;
    }

    header { min-height: 100vh; display: flex; flex-direction: column; justify-content: center; align-items: center; position: relative; }
    .snow { position: fixed; top:0; left:0; width:100%; height:100%; pointer-events:none; z-index:1; }

    section { padding: 80px 5%; }
    h2 { font-size: clamp(2rem, 7vw, 3rem); margin-bottom: 50px; }

    .works {
      grid-template-columns: 1fr;
      gap: 25px;
    }
    @media (min-width: 640px) {
      .works { grid-template-columns: repeat(auto-fit, minmax(340px,1fr)); }
    }

    .card:hover {
      transform: translateY(-10px); /* на мобиле hover почти не работает, зато работает при тапе */
    }
    .card:active {
      transform: translateY(-15px) scale(0.98);
      box-shadow: 0 25px 50px rgba(0,255,255,0.3) !important;
    }

    .neon-btn {
      padding: 16px 40px;
      font-size: 1.1rem;
      touch-action: manipulation;
    }

    /* Снег на мобиле чуть меньше и реже — чтобы не жрал батарею */
    @media (max-width: 768px) {
      .snowflake {
        font-size: 12px !important;
      }
    }
  </style>
</head>
<body>
  <!-- весь HTML из предыдущего сообщения остаётся ТОЧНО ТАКИМ ЖЕ -->
  <!-- просто вставь сюда header, section, footer и скрипты из прошлого ответа -->

  <script>
    // Снег адаптивный и экономит батарею на мобиле
    const snowContainer = document.getElementById('snow');
    const snowflakes = '❄✺✼❅❆';
    const isMobile = window.innerWidth <= 768;
    const interval = isMobile ? 500 : 200;        // на мобиле реже
    const maxFlakes = isMobile ? 30 : 80;         // и меньше снежинок одновременно

    let flakeCount = 0;
    const createFlake = () => {
      if (flakeCount >= maxFlakes) return;
      flakeCount++;

      const flake = document.createElement('div');
      flake.classList.add('snowflake');
      flake.textContent = snowflakes[Math.floor(Math.random()*snowflakes.length)];
      flake.style.left = Math.random() * 100 + 'vw';
      flake.style.animationDuration = Math.random() * 8 + 7 + 's';
      flake.style.fontSize = (isMobile ? Math.random()*10 + 8 : Math.random()*20 + 10) + 'px';
      flake.style.opacity = Math.random() * 0.7 + 0.3;
      snowContainer.appendChild(flake);

      flake.addEventListener('animationend', () => {
        flake.remove();
        flakeCount--;
      });
    };

    setInterval(createFlake, interval);

    // Пасхалка Konami Code — на телефоне тоже работает!
    // Просто тапай по экрану в нужной последовательности:
    // Вверх ↑ Вверх ↑ Вниз ↓ Вниз ↓ Влево ← Вправо → Влево ← Вправо → B A
    let konamiCode = [];
    const secret = [38,38,40,40,37,39,37,39,66,65];

    // Поддержка и клавиатуры, и тачей (свайпы)
    document.addEventListener('keydown', (e) => {
      konamiCode.push(e.keyCode);
      if (konamiCode.length > secret.length) konamiCode.shift();
      if (konamiCode.toString() === secret.toString()) activateEasterEgg();
    });

    // Свайпы для мобильных
    let touchStartY = 0, touchStartX = 0;
    document.addEventListener('touchstart', e => {
      touchStartX = e.touches[0].clientX;
      touchStartY = e.touches[0].clientY;
    });
    document.addEventListener('touchend', e => {
      if (!touchStartX || !touchStartY) return;
      let diffX = touchStartX - e.changedTouches[0].clientX;
      let diffY = touchStartY - e.changedTouches[0].clientY;

      let direction;
      if (Math.abs(diffX) > Math.abs(diffY)) {
        direction = diffX > 0 ? 37 : 39; // ← →
      } else {
        direction = diffY > 0 ? 38 : 40; // ↑ ↓
      }

      konamiCode.push(direction);
      if (konamiCode.length > secret.length) konamiCode.shift();
      if (konamiCode.toString() === secret.toString()) activateEasterEgg();

      touchStartX = 0; touchStartY = 0;
    });

    // Для букв B и A на мобиле просто два быстрых двойных тапа по центру экрана
    let taps = 0;
    document.addEventListener('touchstart', e => {
      if (e.touches.length === 1) {
        taps++;
        setTimeout(() => taps = 0, 800);
        if (taps === 4) { // 4 быстрых тапа = B A
          konamiCode.push(66, 65);
          if (konamiCode.toString().includes('66,65')) activateEasterEgg();
        }
      }
    });

    function activateEasterEgg() {
      document.querySelector('.konami').style.display = 'block';
      document.body.style.overflow = 'hidden';
      // Конфетти-бонус на мобиле
      alert('ПОЗДРАВЛЯЮ! Ты нашёл пасхалку даже с телефона! ');
    }
  </script>
</body>
</html># -
