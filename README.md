<!DOCTYPE html>
<html lang="ja">
<img src=""https://raw.githubusercontent.com/goldfish925/numbers4-predictor/main/cat.png">" alt="にゃんこ">
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ナンバーズ4予測するにゃ！</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #fffaf0;
      color: #333;
      padding: 20px;
    }
    h1 {
      color: #d2691e;
    }
    button {
      background-color: #f4a460;
      border: none;
      padding: 10px 20px;
      font-size: 18px;
      border-radius: 10px;
      cursor: pointer;
      color: white;
    }
    table {
      border-collapse: collapse;
      width: 100%;
      margin-top: 20px;
    }
    th, td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: center;
    }
    #prediction {
      font-size: 24px;
      color: #ff4500;
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h1>ナンバーズ4予測するにゃ！🐾</h1>
  <button onclick="predict()">予測してみる</button>
  <div id="prediction">ここに予測が表示されるにゃ</div>
  <input type="file" id="fileInput" accept=".csv" />
  <table id="dataTable"></table>

  <script>
    let numbers = [];

    document.getElementById('fileInput').addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (!file) return;

      const reader = new FileReader();
      reader.onload = function(e) {
        const csv = e.target.result;
        const lines = csv.split('\n').slice(1);
        const table = document.getElementById('dataTable');
        table.innerHTML = "<tr><th>開催回</th><th>開催日</th><th>抽選数字</th></tr>";
        numbers = [];

        lines.forEach(line => {
          const cols = line.split(',');
          if (cols.length > 2) {
            const draw = cols[2].replace(/="|"/g, '').trim();
            if (draw.length === 4 && !isNaN(draw)) {
              numbers.push(draw);
              const row = `<tr><td>${cols[0]}</td><td>${cols[1]}</td><td>${draw}</td></tr>`;
              table.innerHTML += row;
            }
          }
        });
      };
      reader.readAsText(file, 'Shift_JIS');
    });

    function predict() {
      if (numbers.length === 0) {
        document.getElementById('prediction').innerText = '先にCSVを読み込んでにゃ！';
        return;
      }

      let digitFreq = Array(10).fill(0);
      numbers.forEach(num => {
        num.split('').forEach(d => digitFreq[parseInt(d)]++);
      });

      const topDigits = [...digitFreq.entries()]
        .sort((a, b) => b[1] - a[1])
        .slice(0, 5)
        .map(pair => pair[0].toString());

      let prediction = '';
      for (let i = 0; i < 4; i++) {
        prediction += topDigits[Math.floor(Math.random() * topDigits.length)];
      }

      document.getElementById('prediction').innerText = `にゃんこの予測は… ${prediction} にゃ！`;
    }
  </script>
</body>
</html>
