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
    .resultados, .estimativa {
      margin-top: 20px;
    }
  </style>
</head>
<body>
  <h2>Calculadora de Valor por KM</h2>

  <label for="precoCombustivel">Preço do Combustível (R$/litro):</label>
  <input type="number" id="precoCombustivel" step="0.01" required>

  <label for="consumo">Consumo do Carro (km por litro):</label>
  <input type="number" id="consumo" step="0.1" required>

  <button onclick="calcular()">Calcular Valor do KM</button>

  <div class="resultados" id="resultados" style="display:none;">
    <p><strong>Custo por km:</strong> R$ <span id="valorKm">0.00</span></p>
    <h3>Aceitar Corridas</h3>
    <button onclick="mostrarResultado('minimo')">1. KMmínimo (x3)</button>
    <button onclick="mostrarResultado('medio')">2. KMmédio (x3,5)</button>
    <button onclick="mostrarResultado('ideal')">3. KMideal (x4)</button>
    <button onclick="mostrarResultado('ideal')">Mostrar Melhor Opção</button>
    <h3 id="resultadoFinal"></h3>
  </div>

  <div class="estimativa">
    <h3>Estimar Valor da Corrida (opcional)</h3>
    <label for="distancia">Distância da Corrida (km):</label>
    <input type="number" id="distancia" step="0.1">
    <button onclick="estimarCorrida()">Estimar
