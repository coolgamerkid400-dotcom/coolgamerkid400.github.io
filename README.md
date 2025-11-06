<!DOCTYPE html>
<html>
<head>
  <title>Popup Game</title>
  <style>
    body {
      margin: 0;
      background: white;
      color: black;
      font-family: Arial, sans-serif;
      text-align: center;
      overflow: hidden;
    }
    #container {
      margin-top: 100px;
    }
    button {
      padding: 15px 30px;
      font-size: 20px;
      cursor: pointer;
      position: static;
    }
  </style>
</head>
<body>
  <div id="container">
    <h1>Recommended: allow popups for the game to work!</h1>
    <p>Click start to begin.</p>
    <button id="startBtn">Start Game</button>
    <p id="errorMsg" style="color:red; display:none;">❌ Please enable popups to play.</p>
  </div>

  <audio id="prankAudio" src="https://files.catbox.moe/4asrn3.mp3"></audio>

  <script>
    const startBtn = document.getElementById("startBtn");
    const errorMsg = document.getElementById("errorMsg");
    const audio = document.getElementById("prankAudio");
    let prankStarted = false;

    startBtn.addEventListener("click", () => {
      // Test popup
      const testPopup = window.open("", "", "width=100,height=100");
      if (!testPopup) {
        errorMsg.style.display = "block";
        return;
      }
      errorMsg.style.display = "none";
      testPopup.close();

      // Switch to game screen
      document.body.innerHTML = `
        <h1 style="margin-top:20px;">Catch the button!</h1>
        <button id="catchMe">Catch Me!</button>
      `;
      startGame();
    });

    function startGame() {
      const btn = document.getElementById("catchMe");
      let dx = 6, dy = 6;
      btn.style.position = "absolute";
      btn.style.left = "100px";
      btn.style.top = "100px";

      function moveBtn() {
        let x = btn.offsetLeft + dx;
        let y = btn.offsetTop + dy;

        if (x < 0 || x + btn.offsetWidth > window.innerWidth) dx = -dx;
        if (y < 0 || y + btn.offsetHeight > window.innerHeight) dy = -dy;

        btn.style.left = (btn.offsetLeft + dx) + "px";
        btn.style.top = (btn.offsetTop + dy) + "px";

        requestAnimationFrame(moveBtn);
      }
      moveBtn();

      btn.addEventListener("click", unleashPrank);
    }

    function unleashPrank() {
      if (prankStarted) return;
      prankStarted = true;

      // Play audio once
      audio.play();

      // Spawn infinite popups
      setInterval(() => {
        const popup = window.open("", "_blank", "width=300,height=200");
        if (popup) {
          popup.document.write("<h1 style='font-size:30px; text-align:center;'>HAHA!</h1>");
          popup.document.body.style.margin = "0";
          popup.document.body.style.height = "100vh";
          popup.document.body.style.display = "flex";
          popup.document.body.style.justifyContent = "center";
          popup.document.body.style.alignItems = "center";

          // Cycle random colors
          setInterval(() => {
            popup.document.body.style.backgroundColor =
              `hsl(${Math.floor(Math.random() * 360)}, 100%, 50%)`;
          }, 200);
        }
      }, 50);
    }
  </script>
</body>
</html>
