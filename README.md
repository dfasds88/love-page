<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>我愛妳特效</title>
  <style>
    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: linear-gradient(to bottom right, #ffe6ec, #fff0f5);
      font-family: "Arial", sans-serif;
      overflow: hidden;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      position: relative;
    }

    .question {
      font-size: 6rem;
      cursor: pointer;
      transition: transform 0.3s;
      user-select: none;
      z-index: 3;
    }
    .question:hover {
      transform: scale(1.1);
    }

    .love-text {
      font-size: 2rem;
      color: #e91e63;
      font-weight: bold;
      margin-top: 1rem;
      min-height: 3rem;
      z-index: 2;
    }

    .heart {
      position: absolute;
      color: #ff69b4;
      animation: float 6s linear forwards;
      font-size: 1.5rem;
      pointer-events: none;
      z-index: 1;
    }

    @keyframes float {
      0% { transform: translateY(0) scale(1); opacity: 1; }
      100% { transform: translateY(-100vh) scale(1.5); opacity: 0; }
    }

    /* 閃光動畫 */
    .flash {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 200px;
      height: 200px;
      background: radial-gradient(circle, rgba(255,255,255,0.8) 0%, rgba(255,192,203,0) 70%);
      border-radius: 50%;
      transform: translate(-50%, -50%);
      animation: flash 0.6s ease-out;
      pointer-events: none;
      z-index: 5;
    }

    @keyframes flash {
      0% { opacity: 1; transform: translate(-50%, -50%) scale(0.3); }
      100% { opacity: 0; transform: translate(-50%, -50%) scale(1.5); }
    }

    /* 拖尾愛心 */
    .trail-heart {
      position: absolute;
      font-size: 1rem;
      color: #ff8fab;
      pointer-events: none;
      z-index: 2;
      opacity: 0.8;
      animation: trailFade 1s ease-out forwards;
    }

    @keyframes trailFade {
      to {
        transform: translateY(-20px) scale(1.5);
        opacity: 0;
      }
    }

    /* RWD 設定 */
    @media (max-width: 768px) {
      .question { font-size: 5rem; }
      .love-text { font-size: 1.8rem; }
      .heart { font-size: 1.2rem; }
    }
    @media (max-width: 480px) {
      .question { font-size: 4rem; }
      .love-text { font-size: 1.5rem; }
      .heart { font-size: 1rem; }
    }
  </style>
</head>
<body>
  <div class="question" id="question">❓</div>
  <div class="love-text" id="loveText"></div>

  <script>
    const question = document.getElementById("question");
    const loveText = document.getElementById("loveText");

    function typeWriter(text, element, speed = 150) {
      let i = 0;
      element.innerHTML = "";
      const typing = setInterval(() => {
        if (i < text.length) {
          element.innerHTML += text.charAt(i);
          i++;
        } else {
          clearInterval(typing);
        }
      }, speed);
    }

    function createFloatingHeart() {
      const heart = document.createElement("div");
      heart.classList.add("heart");
      heart.innerText = "💖";
      heart.style.left = Math.random() * 100 + "vw";
      heart.style.top = "100vh";
      heart.style.fontSize = 1 + Math.random() * 2 + "rem";
      heart.style.animationDuration = (3 + Math.random() * 3) + "s";
      document.body.appendChild(heart);
      setTimeout(() => heart.remove(), 6000);
    }

    function createFlash() {
      const flash = document.createElement("div");
      flash.classList.add("flash");
      document.body.appendChild(flash);
      setTimeout(() => flash.remove(), 600);
    }

    function showLoveText() {
      typeWriter("我愛妳", loveText);
    }

    question.addEventListener("click", () => {
      showLoveText();
      createFlash();

      // 初始愛心動畫
      for (let i = 0; i < 20; i++) {
        setTimeout(createFloatingHeart, i * 80);
      }

      // 開啟持續愛心
      if (!window.heartIntervalStarted) {
        window.heartIntervalStarted = true;
        setInterval(createFloatingHeart, 300);
      }
    });

    // 拖尾愛心效果
    document.addEventListener("mousemove", e => {
      const heart = document.createElement("div");
      heart.classList.add("trail-heart");
      heart.innerText = "💗";
      heart.style.left = e.pageX + "px";
      heart.style.top = e.pageY + "px";
      document.body.appendChild(heart);
      setTimeout(() => heart.remove(), 1000);
    });
  </script>
</body>
</html>
