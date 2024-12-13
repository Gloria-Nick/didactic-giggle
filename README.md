<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>我们的Emoji小花园</title>
<style>
  body {
    font-family: sans-serif;
    background: linear-gradient(to bottom, #e0f7fa, #b2ebf2);
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    overflow: hidden;
  }
  h1 {
    color: #00796b;
    text-shadow: 2px 2px 4px #4db6ac;
  }
  .garden {
    width: 90%;
    min-height: 300px;
    border: 3px dashed #4db6ac;
    margin-top: 20px;
    padding: 10px;
    background: #ffffff88;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  }
  .flower {
    font-size: 2rem;
    margin: 10px;
    cursor: pointer;
    transition: transform 0.2s, box-shadow 0.2s;
    padding: 5px;
    border-radius: 50%;
  }
  .flower:hover {
    transform: scale(1.3);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  }
  #add-form {
    margin: 20px 0;
  }
  #flower-input {
    font-size: 1rem;
    padding: 5px;
    margin-right: 10px;
    border: 1px solid #4db6ac;
    border-radius: 5px;
  }
  #add-button {
    font-size: 1rem;
    padding: 5px 10px;
    background: #4db6ac;
    border: none;
    cursor: pointer;
    color: #fff;
    border-radius: 5px;
    transition: background 0.3s;
  }
  #add-button:hover {
    background: #00796b;
  }
  #flower-count {
    margin-top: 10px;
    color: #00796b;
    font-size: 1.2rem;
    font-weight: bold;
  }
</style>
</head>
<body>
  <h1>我们的Emoji小花园 🌸</h1>
  <p>你我共同的花园，每一朵花都是爱的见证</p>
  <div id="add-form">
    <input id="flower-input" placeholder="请输入emoji花花或它的意义" />
    <button id="add-button">种花</button>
  </div>
  <div id="flower-count"></div>
  <div class="garden" id="garden"></div>

  <!-- Firebase JS SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

  <script>
    const firebaseConfig = {
      apiKey: "AIzaSyC80oJXAtAM9z7MWrPasnKefREJKF2a4KE",
      authDomain: "emoji-garden-7b65f.firebaseapp.com",
      databaseURL: "https://emoji-garden-7b65f-default-rtdb.firebaseio.com",
      projectId: "emoji-garden-7b65f",
      storageBucket: "emoji-garden-7b65f.firebasestorage.app",
      messagingSenderId: "944937277557",
      appId: "1:944937277557:web:97b4474d33a745f335c03c",
      measurementId: "G-Q1RSEJ056M",
    };

    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();
    const gardenRef = db.ref("garden");

    const garden = document.getElementById("garden");
    const addButton = document.getElementById("add-button");
    const flowerInput = document.getElementById("flower-input");
    const flowerCount = document.getElementById("flower-count");

    // 点击添加花朵
    addButton.addEventListener("click", () => {
      const emoji = flowerInput.value.trim();
      if (emoji) {
        const flowerData = {
          emoji,
          date: new Date().toLocaleString(),
        };
        gardenRef.push(flowerData);
        flowerInput.value = "";
      }
    });

    // 实时监听数据库更新
    gardenRef.on("value", (snapshot) => {
      garden.innerHTML = "";
      const flowers = snapshot.val();
      if (flowers) {
        const flowerArray = Object.values(flowers);
        flowerCount.textContent = `你已经种了 ${flowerArray.length} 朵花！`;
        flowerArray.forEach((flower) => {
          const flowerEl = document.createElement("div");
          flowerEl.className = "flower";
          flowerEl.textContent = flower.emoji;
          flowerEl.title = `种下时间：${flower.date}`;
          flowerEl.addEventListener("click", () => {
            alert(`这朵花的意义：${flower.emoji}\n种下时间：${flower.date}`);
          });
          garden.appendChild(flowerEl);
        });
      } else {
        flowerCount.textContent = "花园里还没有花，快来种下第一朵吧！";
      }
    });
  </script>
</body>
</html>
