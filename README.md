<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Motorista Pro KM</title>
  <link rel="manifest" href="manifest.json">
  <meta name="theme-color" content="#121212">
  <style>
    body {
      background-color: #121212;
      color: #FFFFFF;
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      padding: 20px;
      max-width: 480px;
      margin: auto;
    }

    h2 {
      text-align: center;
      color: #00E676;
      margin-bottom: 30px;
    }

    label {
      font-size: 14px;
      margin-bottom: 5px;
      display: block;
    }

    input {
      width: 100%;
      padding: 14px;
      margin-bottom: 20px;
      border: none;
      border-radius: 8px;
      background-color: #1e1e1e;
      color: white;
      font-size: 16px;
    }

    button {
      width: 100%;
      padding: 14px;
      margin-bottom: 12px;
      border: none;
      border-radius: 8px;
      background-color: #00E676;
      color: #000;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #00c867;
    }

    .secao {
      margin-bottom: 40px;
    }

    .resultados, .estimativa {
      background-color: #1e1e1e;
      padding: 20px;
      border-radius: 12px;
    }

    .botao-opcao {
      background-color: #333;
      color: #fff;
    }

    .botao-opcao:hover {
      background-color: #555;
    }

    #resultadoFinal {
      margin-top: 20px;
      font-size: 18px;
      color: #00E676;
      text-align: center;
    }

    #estimativaResultado {
      margin-top: 10px;
      text-align: center;
      font-size: 14px;
      color: #ccc;
    }
  </style>
</head>
<body>

  <h2>Motorista Pro KM</h2>

  <div class="secao">
    <label for="precoCombustivel">Preço do Combustível (R$/litro):</label>
    <input type="number" id="precoCombustivel" step="0.01">

    <label for="consumo">Consumo do Carro (km/litro):</label>
    <input type="number" id="consumo" step="0.1">

    <button onclick="calcular()">Calcular Valor do KM</button>
  </div>

  <div class="resultados" id="resultados" style="display:none;">
    <p><strong>Custo por km:</strong> R$ <span id="valorKm">0.00</span></p>

    <h3 style="margin-top: 20px;">Aceitar Corridas</h3>
    <button class="botao-opcao" onclick="mostrarResultado('minimo')">1. KMmínimo (x3)</button>
    <button class="botao-opcao" onclick="mostrarResultado('medio')">2. KMmédio (x3,5)</button>
    <button class="botao-opcao" onclick="mostrarResultado('ideal')">3. KMideal (x4)</button>
    <button onclick="mostrarResultado('ideal')">Mostrar Melhor Opção</button>

    <h3 id="resultadoFinal"></h3>
  </div>

  <div class="estimativa">
    <h3>Estimar Valor da Corrida (opcional)</h3>
    <label for="distancia">Distância da Corrida (km):</label>
    <input type="number" id="distancia" step="0.1">
    <button onclick="estimarCorrida()">Estimar Corrida</button>
    <p id="estimativaResultado"></p>
  </div>

  <script>
    let valorKmBase = 0;

    function calcular() {
      const preco = parseFloat(document.getElementById('precoCombustivel').value);
      const consumo = parseFloat(document.getElementById('consumo').value);

      if (!isNaN(preco) && preco > 0 && !isNaN(consumo) && consumo > 0) {
        valorKmBase = preco / consumo;
        document.getElementById('valorKm').innerText = valorKmBase.toFixed(2);
        document.getElementById('resultados').style.display = 'block';
        document.getElementById('resultadoFinal').innerText = '';
      } else {
        alert('Preencha os campos corretamente.');
      }
    }

    function mostrarResultado(tipo) {
      if (valorKmBase === 0) {
        alert('Você precisa calcular o valor por KM primeiro.');
        return;
      }

      let multiplicador = 0;
      let titulo = '';

      switch (tipo) {
        case 'minimo': multiplicador = 3; titulo = 'Valor KMmínimo'; break;
        case 'medio': multiplicador = 3.5; titulo = 'Valor KMmédio'; break;
        case 'ideal': multiplicador = 4; titulo = 'Valor KMideal'; break;
      }

      const valorFinal = valorKmBase * multiplicador;
      document.getElementById('resultadoFinal').innerText = `${titulo}: R$ ${valorFinal.toFixed(2)}`;
    }

    function estimarCorrida() {
      const distancia = parseFloat(document.getElementById('distancia').value);
      if (isNaN(distancia) || distancia <= 0 || valorKmBase === 0) {
        alert('Informe uma distância válida e calcule o custo por km antes.');
        return;
      }

      const kmMin = (valorKmBase * 3 * distancia).toFixed(2);
      const kmMed = (valorKmBase * 3.5 * distancia).toFixed(2);
      const kmIde = (valorKmBase * 4 * distancia).toFixed(2);

      document.getElementById('estimativaResultado').innerText =
        `KMmínimo: R$ ${kmMin} | KMmédio: R$ ${kmMed} | KMideal: R$ ${kmIde}`;
    }
  </script>
</body>
</html>
