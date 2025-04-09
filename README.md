<html lang="pt-br">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Calcula KM</title>
  <style>
    body {
      background: #f5f5f5;
      font-family: 'Segoe UI', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .container {
      background: #ffffff;
      padding: 30px;
      border-radius: 15px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.1);
      max-width: 400px;
      width: 100%;
    }

    h2 {
      text-align: center;
      color: #2c3e50;
    }

    label {
      display: block;
      margin-top: 15px;
      color: #555;
    }

    input {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border: 1px solid #ccc;
      border-radius: 8px;
    }

    button {
      margin-top: 20px;
      width: 100%;
      padding: 12px;
      background-color: #2980b9;
      color: white;
      font-weight: bold;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #1c5980;
    }

    .resultado {
      margin-top: 20px;
      text-align: center;
      color: #27ae60;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Calcula KM</h2>
    <label for="km">Quilômetros rodados:</label>
    <input type="number" id="km" placeholder="Ex: 100">

    <label for="gasto">Gasto com Combustivel (R$):</label>
    <input type="number" id="gasto" placeholder="Ex: 50">

    <button onclick="calcular()">Calcular</button>

    <div class="resultado" id="resultado"></div>
  </div>

  <script>
    function calcular() {
      const km = parseFloat(document.getElementById("km").value);
      const gasto = parseFloat(document.getElementById("gasto").value);

      if (isNaN(km) || isNaN(gasto) || km === 0) {
        document.getElementById("resultado").innerText = "Preencha os campos corretamente.";
        return;
      }

      const valorPorKm = gasto / km;
      document.getElementById("resultado").innerText = `Valor por KM: R$ ${valorPorKm.toFixed(2)}`;
    }
  </script>
</body>
</html>
