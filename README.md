<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Project X — Вложение в BNB</title>
  <script src="https://cdn.jsdelivr.net/npm/web3@1.7.6/dist/web3.min.js"></script>
  <style>
    body {
      font-family: sans-serif;
      background: #111;
      color: #fff;
      padding: 30px;
    }
    input, select, button {
      padding: 10px;
      margin: 10px 0;
      width: 100%;
      max-width: 300px;
      font-size: 16px;
    }
    .success {
      color: #0f0;
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <h1>Вложение в Project X</h1>

  <label>Сумма (в BNB):</label><br>
  <input type="number" id="amount" step="0.001" placeholder="Например, 0.05"><br>

  <label>Выберите опцию:</label><br>
  <select id="lockOption">
    <option value="0">🔓 Без блокировки</option>
    <option value="6">🔒 С блокировкой на 6 месяцев</option>
    <option value="12">🔒 С блокировкой на 12 месяцев</option>
  </select><br>

  <button onclick="sendBNB()">Отправить</button>

  <div class="success" id="successMsg"></div>

  <script>
    const contractAddress = "0x7bB165Bf345B24dee9104D9d9b43411d6838D950";

    async function sendBNB() {
      if (typeof window.ethereum === 'undefined') {
        alert('Установите MetaMask!');
        return;
      }

      const web3 = new Web3(window.ethereum);
      const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
      const from = accounts[0];

      const amount = parseFloat(document.getElementById("amount").value);
      const lock = document.getElementById("lockOption").value;

      if (!amount || amount <= 0) {
        alert("Введите корректную сумму.");
        return;
      }

      const valueWei = web3.utils.toWei(amount.toString(), "ether");

      try {
        await web3.eth.sendTransaction({
          from,
          to: contractAddress,
          value: valueWei
        });

        document.getElementById("successMsg").innerText = "Транзакция прошла успешно, поздравляю!";

        // можно здесь добавить запись о типе блокировки, если ты будешь потом её обрабатывать

      } catch (e) {
        alert("Ошибка: " + e.message);
      }
    }
  </script>

</body>
</html>
