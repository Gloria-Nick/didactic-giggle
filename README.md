<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>我们的Emoji小花园</title>
<style>
  body {
    font-family: sans-serif;
    background: #e0f7fa; 
    display: flex; 
    flex-direction: column; 
    align-items: center; 
    padding: 20px;
  }
  h1 {
    color: #00796b;
  }
  .garden {
    width: 90%;
    min-height: 300px;
    border: 3px dashed #4db6ac;
    margin-top: 20px;
    padding: 10px;
    background: #b2dfdb;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: center;
  }
  .flower {
    font-size: 2rem;
    margin: 10px;
    cursor: pointer;
    transition: transform 0.2s;
  }
  .flower:hover {
    transform: scale(1.3);
  }
  #add-form {
    margin: 20px 0;
  }
  #flower-input {
    font-size: 1rem;
    padding: 5px;
    margin-right: 10px;
  }
  #add-button {
    font-size: 1rem;
    padding: 5px 10px;
    background: #4db6ac;
    border: none;
    cursor: pointer;
    color: #fff;
    border-radius: 5px;
  }
</style>
</head>
<body>
  <h1>我们的Emoji小花园 🌸</h1>
  <p>你我共同的花园，每一朵花都是爱的见证</p>
  <div id="add-form">
    <input id="flower-input" placeholder="请输入emoji花花"/>
    <button id="add-button">种花</button>
  </div>
  <div class="garden" id="garden"></div>

  <!-- Firebase JS SDKs -->
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

  <script>
    // 使用你的实际firebaseConfig
    const firebaseConfig = {
      apiKey: "AIzaSyC80oJXAtAM9z7MWrPasnKefREJKF2a4KE",
  authDomain: "emoji-garden-7b65f.firebaseapp.com",
  databaseURL: "https://emoji-garden-7b65f-default-rtdb.firebaseio.com",
  projectId: "emoji-garden-7b65f",
  storageBucket: "emoji-garden-7b65f.firebasestorage.app",
  messagingSenderId: "944937277557",
  appId: "1:944937277557:web:97b4474d33a745f335c03c",
  measurementId: "G-Q1RSEJ056M"
    };

    // 初始化Firebase (compat写法)
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();
    const gardenRef = db.ref('garden'); // 数据库存放花朵的位置

    const garden = document.getElementById('garden');
    const addButton = document.getElementById('add-button');
    const flowerInput = document.getElementById('flower-input');

    // 点击添加花朵
    addButton.addEventListener('click', () => {
      const emoji = flowerInput.value.trim();
      if(emoji) {
        // 推送到数据库
        gardenRef.push(emoji);
        flowerInput.value = '';
      }
    });

    // 实时监听数据库更新，显示花朵
    gardenRef.on('value', (snapshot) => {
      garden.innerHTML = '';
      const flowers = snapshot.val();
      if (flowers) {
        Object.values(flowers).forEach(flowerEmoji => {
          const flowerEl = document.createElement('div');
          flowerEl.className = 'flower';
          flowerEl.textContent = flowerEmoji;
          flowerEl.title = '为你而种';
          flowerEl.addEventListener('click', () => {
            alert('这朵花是为你而种的哦！💖');
          });
          garden.appendChild(flowerEl);
        });
      }
    });
  </script>
</body>
</html>
