<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Calculadora de Custo por KM</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 400px;
      margin: auto;
      padding: 20px;
    }
    label, input {
      display: block;
      margin: 10px 0;
    }
    button {
      margin-top: 10px;
      padding: 10px;
      width: 100%;
      font-size: 16px;
    }
    .resultados {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h2>Calculadora de Valor por KM</h2>

  <label for="precoCombustivel">Preço do Combustível (R$/litro):</label>
  <input type="number" id="precoCombustivel" step="0.01">

  <label for="consumo">Consumo do Carro (km por litro):</label>
  <input type="number" id="consumo" step="0.1">

  <button onclick="calcular()">Calcular Valor por KM</button>

  <div class="resultados" id="resultados" style="display:none;">
    <p><strong>Custo por km:</strong> R$ <span id="valorKm">0.00</span></p>
    <button onclick="mostrarResultado('minimo')">1. KMmínimo (x3)</button>
    <button onclick="mostrarResultado('medio')">2. KMmédio (x3,5)</button>
    <button onclick="mostrarResultado('ideal')">3. KMideal (x4)</button>

    <h3 id="resultadoFinal"></h3>
  </div>

  <script>
    let valorKmBase = 0;

    function calcular() {
      const preco = parseFloat(document.getElementById('precoCombustivel').value);
      const consumo = parseFloat(document.getElementById('consumo').value);

      if (preco > 0 && consumo > 0) {
        valorKmBase = preco / consumo;
        document.getElementById('valorKm').innerText = valorKmBase.toFixed(2);
        document.getElementById('resultados').style.display = 'block';
        document.getElementById('resultadoFinal').innerText = '';
      } else {
        alert('Preencha todos os campos corretamente.');
      }
    }

    function mostrarResultado(tipo) {
      let multiplicador = 0;
      let titulo = '';

      switch (tipo) {
        case 'minimo':
          multiplicador = 3;
          titulo = 'Valor KMmínimo';
          break;
        case 'medio':
          multiplicador = 3.5;
          titulo = 'Valor KMmédio';
          break;
        case 'ideal':
          multiplicador = 4;
          titulo = 'Valor KMideal';
          break;
      }

      const valorFinal = valorKmBase * multiplicador;
      document.getElementById('resultadoFinal').innerText = `${titulo}: R$ ${valorFinal.toFixed(2)}`;
    }
  </script>
</body>
</html>
