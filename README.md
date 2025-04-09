<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Melhorando os Ganhos</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #f5f5f5;
      margin: 0;
      padding: 20px;
      color: #333;
      text-align: center;
    }

    .container {
      max-width: 500px;
      margin: auto;
      background-color: #fff;
      padding: 25px;
      border-radius: 12px;
      border: 2px solid #000;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    }

    h2 {
      margin-top: 0;
      color: #007bff;
      font-size: 26px;
      font-weight: bold;
      text-align: center;
    }

    label {
      font-weight: 600;
      display: block;
      margin-top: 15px;
      text-align: left;
    }

    input {
      width: 100%;
      padding: 12px;
      margin-top: 5px;
      margin-bottom: 15px;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
    }

    button {
      width: 100%;
      padding: 14px;
      background-color: #2e8b57;
      color: white;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      cursor: pointer;
      margin-top: 10px;
    }

    button:hover {
      background-color: #256f47;
    }

    .resultados {
      margin-top: 20px;
    }

    .popup-overlay {
      display: none;
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background-color: rgba(0,0,0,0.4);
      justify-content: center;
      align-items: center;
      z-index: 999;
    }

    .popup {
      background-color: white;
      padding: 25px;
      border-radius: 12px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.2);
      max-width: 300px;
      text-align: center;
    }

    .popup button {
      margin-top: 15px;
      padding: 10px 20px;
    }

    .estimativa {
      margin-top: 30px;
      padding: 20px;
      background-color: #f9f9f9;
      border-radius: 12px;
    }

    .estimativa h3 {
      margin-top: 0;
      font-size: 18px;
    }

    #valorKm {
      color: #007bff;
      font-weight: bold;
    }

    #estimativaResultado {
      margin-top: 10px;
      text-align: center;
      font-size: 14px;
      color: #555;
    }
  </style>
</head>
<body>

  <div class="container">
    <h2>CALCULO-DO-MOTORISTA</h2>

    <label for="precoCombustivel">Preço do combustível (R$/litro):</label>
    <input type="number" id="precoCombustivel" step="0.01" />

    <label for="consumo">Consumo do carro (km/litro):</label>
    <input type="number" id="consumo" step="0.1" />

    <button onclick="calcular()">Calcular Valor do KM</button>

    <div class="resultados" id="resultados" style="display: none;">
      <p><strong>Custo por km:</strong> <span id="valorKm">R$ 0.00</span></p>

      <h3>Aceitar Corridas</h3>
      <button onclick="mostrarResultado('minimo')">1. KMmínimo (x3)</button>
      <button onclick="mostrarResultado('medio')">2. KMmédio (x3,5)</button>
      <button onclick="mostrarResultado('ideal')">3. KMideal (x4)</button>
    </div>

    <div class="estimativa">
      <h3>Calcular Corrida Particular</h3>
      <label for="distancia">Distância da Corrida (km):</label>
      <input type="number" id="distancia" step="0.1" />
      <button onclick="estimarCorrida()">Calcular Corrida Particular</button>
      <p id="estimativaResultado"></p>
    </div>
  </div>

  <div class="popup-overlay" id="popupOverlay">
    <div class="popup">
      <h3 id="popupTitulo">Resultado</h3>
      <p id="popupTexto"></p>
      <button onclick="fecharPopup()">Fechar</button>
    </div>
  </div>

  <script>
    let valorKmBase = 0;

    function calcular() {
      const preco = parseFloat(document.getElementById('precoCombustivel').value);
      const consumo = parseFloat(document.getElementById('consumo').value);

      if (!isNaN(preco) && preco > 0 && !isNaN(consumo) && consumo > 0) {
        valorKmBase = preco / consumo;
        document.getElementById('valorKm').innerText = `R$ ${valorKmBase.toFixed(2)}`;
        document.getElementById('resultados').style.display = 'block';
      } else {
        alert('Preencha os campos corretamente.');
      }
    }

    function mostrarResultado(tipo) {
      if (valorKmBase === 0) {
        alert('Calcule primeiro o valor do km.');
        return;
      }

      let multiplicador = 0;
      let titulo = '';

      switch (tipo) {
        case 'minimo': multiplicador = 3; titulo = 'KMmínimo'; break;
        case 'medio': multiplicador = 3.5; titulo = 'KMmédio'; break;
        case 'ideal': multiplicador = 4; titulo = 'KMideal'; break;
      }

      const valorFinal = valorKmBase * multiplicador;
      document.getElementById('popupTitulo').innerText = titulo;
      document.getElementById('popupTexto').innerText = `R$ ${valorFinal.toFixed(2)}`;
      document.getElementById('popupOverlay').style.display = 'flex';
    }

    function fecharPopup() {
      document.getElementById('popupOverlay').style.display = 'none';
    }

    function estimarCorrida() {
      const distancia = parseFloat(document.getElementById('distancia').value);
      if (isNaN(distancia) || distancia <= 0 || valorKmBase === 0) {
        alert('Preencha corretamente os campos e calcule o valor por km.');
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
