<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Calculadora de Custo por KM</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 500px;
      margin: auto;
      padding: 20px;
    }
    label, input {
      display: block;
      margin: 10px 0;
    }
    button {
      margin: 5px 0;
      padding: 10px;
      width: 100%;
      font-size: 16px;
    }
    .resultados, .estimativa, .historico {
      margin-top: 20px;
    }
    textarea {
      width: 100%;
      height: 100px;
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

  <div class="estimativa">
    <h3>Estimar Valor da Corrida (opcional)</h3>
    <label for="distancia">Distância da Corrida (km):</label>
    <input type="number" id="distancia" step="0.1">
    <button onclick="estimarCorrida()">Estimar Corrida</button>
    <p id="estimativaResultado"></p>
  </div>

  <div class="historico">
    <h3>Histórico de Cálculos</h3>
    <button onclick="adicionarAoHistorico()">Adicionar ao Histórico</button>
    <textarea id="historicoTexto" readonly></textarea><br>
    <button onclick="baixarPDF()">Baixar Histórico em PDF</button>
  </div>

  <!-- Biblioteca para gerar PDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

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
        case 'minimo': multiplicador = 3; titulo = 'Valor KMmínimo'; break;
        case 'medio': multiplicador = 3.5; titulo = 'Valor KMmédio'; break;
        case 'ideal': multiplicador = 4; titulo = 'Valor KMideal'; break;
      }

      const valorFinal = valorKmBase * multiplicador;
      document.getElementById('resultadoFinal').innerText = `${titulo}: R$ ${valorFinal.toFixed(2)}`;
    }

    function estimarCorrida() {
      const distancia = parseFloat(document.getElementById('distancia').value);
      if (valorKmBase === 0 || isNaN(distancia) || distancia <= 0) {
        alert('Informe a distância e calcule o valor por km primeiro.');
        return;
      }

      const kmMin = (valorKmBase * 3 * distancia).toFixed(2);
      const kmMed = (valorKmBase * 3.5 * distancia).toFixed(2);
      const kmIde = (valorKmBase * 4 * distancia).toFixed(2);

      document.getElementById('estimativaResultado').innerText =
        `KMmínimo: R$ ${kmMin} | KMmédio: R$ ${kmMed} | KMideal: R$ ${kmIde}`;
    }

    function adicionarAoHistorico() {
      const preco = document.getElementById('precoCombustivel').value;
      const consumo = document.getElementById('consumo').value;
      const custo = valorKmBase.toFixed(2);
      const linha =
