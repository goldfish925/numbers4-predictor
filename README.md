
<!DOCTYPE html>
<html lang="ja">
<img src="./neco.png" alt="ネコの画像" style="width: 200px; margin: 20px auto; display: block;">
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ナンバーズ4予測にゃ！</title>
  <style>
    body {
      font-family: "Hiragino Kaku Gothic ProN", sans-serif;
      background-color: #fffaf0;
      color: #333;
      text-align: center;
      padding: 20px;
    }
    h1 {
      color: #1e90ff;
    }
    h2 {
      color: #d2691e;
    }
    button {
      background-color: #f4a460;
      color: white;
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      font-size: 16px;
      cursor: pointer;
      margin-top: 10px;
    }
    input[type="file"] {
      margin-top: 10px;
    }
    #result {
      margin-top: 20px;
      font-size: 18px;
      color: #ff4500;
    }
    img {
      max-width: 200px;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>numbers4-predictor</h1>
  <h2>ナンバーズ4予測するにゃ！🐾</h2>
  <img src="https://raw.githubusercontent.com/goldfish925/numbers4-predictor/main/cat.png" alt="にゃんこ" />
  <input type="file" id="csvFile" accept=".csv" style="display:block; margin: 10px auto;">
  <button onclick="predict()">予測してみる</button>
  <div id="result">ここに予測が表示されるにゃ</div>
  <input type="file" id="csvFile" accept=".csv" />

  <script>
    function predict() {
      const fileInput = document.getElementById("csvFile");
      const file = fileInput.files[0];
      if (!file) {
        document.getElementById("result").textContent = "ファイルを選んでにゃ";
        return;
      }

      const reader = new FileReader();
      reader.onload = function (e) {
        const text = e.target.result;
        const lines = text.split("\n").slice(-20); // 最新20件
        const numbers = lines
          .map(line => line.trim().split(",")[1])
          .filter(n => /^\d{4}$/.test(n));
        
        const count = {};
        numbers.forEach(num => {
          for (const digit of num) {
            count[digit] = (count[digit] || 0) + 1;
          }
        });

        const sorted = Object.entries(count).sort((a, b) => b[1] - a[1]);
        const prediction = sorted.slice(0, 4).map(entry => entry[0]).join("");

        document.getElementById("result").textContent = `未来の数字は…「${prediction}」にゃ！✨`;
      };
      reader.readAsText(file);
    }
  </script>
</body>
</html>
