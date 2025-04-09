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
  <input type="number" id="precoCombustivel" step="0.01" required>

  <label for="consumo">Consumo do Carro (km por litro):</label>
  <input type="number" id="consumo" step="0.1" required>

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

  <!-- Biblioteca PDF -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>

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

    function adicionarAoHistorico() {
      if (valorKmBase === 0) {
        alert('Calcule o valor por KM primeiro.');
        return;
      }

      const preco = document.getElementById('precoCombustivel').value;
      const consumo = document.getElementById('consumo').value;
      const custo = valorKmBase.toFixed(2);
      const linha = `Combustível: R$${preco}, Consumo: ${consumo}km/L, Custo/km: R$${custo}\n`;
      document.getElementById('historicoTexto').value += linha;
    }

    function baixarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      const texto = document.getElementById('historicoTexto').value || "Sem dados no histórico.";
      const linhas = texto.split('\n');
      let y = 10;

      linhas.forEach(linha => {
        if (linha.trim() !== '') {
          doc.text(linha, 10, y);
          y += 10;
        }
      });

      doc.save("historico-km.pdf");
    }
  </script>
</body>
</html>
