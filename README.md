<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Melhorando os Ganhos</title>
  <style>
    body {
      background-color: #f7f7f7;
      font-family: 'Segoe UI', sans-serif;
      color: #333;
      margin: 0;
      padding: 20px;
      max-width: 480px;
      margin: auto;
    }

    h2 {
      text-align: center;
      color: #32c766;
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
      border: 1px solid #ddd;
      border-radius: 10px;
      background-color: #fff;
      font-size: 16px;
    }

    button {
      width: 100%;
      padding: 14px;
      margin-bottom: 12px;
      border: none;
      border-radius: 10px;
      background-color: #32c766;
      color: #fff;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: #28b159;
    }

    .botao-opcao {
      background-color: #e0e0e0;
      color: #333;
    }

    .botao-opcao:hover {
      background-color: #ccc;
    }

    .secao {
      margin-bottom: 40px;
    }

    .resultados, .estimativa {
      background-color: #fff;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.08);
    }

    .estimativa h3 {
      margin-top: 0;
    }

    #estimativaResultado {
      margin-top: 10px;
      text-align: center;
      font-size: 14px;
      color: #555;
    }

    .popup-overlay {
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0,0,0,0.4);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 1000;
    }

    .popup {
      background: #fff;
      color: #333;
      padding: 25px;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.2);
      text-align: center;
      max-width: 300px;
      width: 90%;
    }

    .popup h3 {
      margin: 0 0 10px;
      color: #32c766;
    }

    .popup button {
      background-color: #32c766;
      color: white;
      width: auto;
      margin-top: 15px;
      padding: 10px 20px;
    }
  </style>
</head>
<body>

  <h2>Melhorando os Ganhos</h2>

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
  </div>

  <div class="estimativa">
    <h3>Estimar Valor da Corrida (opcional)</h3>
    <label for="distancia">Distância da Corrida (km):</label>
    <input type="number" id="distancia" step="0.1">
    <button onclick="estimarCorrida()">Estimar Corrida</button>
    <p id="estimativaResultado"></p>
  </div>

  <!-- Popup -->
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
        document.getElementById('valorKm').innerText = valorKmBase.toFixed(2);
        document.getElementById('resultados').style.display = 'block';
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
