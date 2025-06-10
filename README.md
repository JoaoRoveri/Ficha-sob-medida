# Ficha-sob-medida
<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ficha Técnica - Di Roveri's</title>
  <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600&family=Montserrat&display=swap" rel="stylesheet">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <style>
    body {
      font-family: 'Montserrat', sans-serif;
      background-color: #f7f7f7;
      color: #333;
      padding: 20px;
    }
    h1 {
      font-family: 'Cinzel', serif;
      color: #D4AF37;
      text-align: center;
      margin-bottom: 10px;
    }
    form {
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      max-width: 900px;
      margin: auto;
    }
    label {
      display: block;
      margin-top: 10px;
      font-weight: 600;
    }
    input, select, textarea {
      width: 100%;
      padding: 8px;
      margin-top: 5px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      background-color: #D4AF37;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      margin-top: 20px;
      cursor: pointer;
      border-radius: 5px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
      gap: 10px;
    }
    .pdf-container {
      display: none;
      padding: 20px;
      background: white;
    }
    .logo {
      font-family: 'Cinzel', serif;
      color: #D4AF37;
      font-size: 24px;
      margin-bottom: 10px;
    }
    .croqui {
      margin-top: 20px;
      width: 100%;
      max-width: 400px;
    }
    @media print {
      button { display: none; }
    }
  </style>
</head>
<body>
  <h1>Ficha Técnica - Di Roveri's</h1>
  <form id="fichaForm">
    <label>Nome do cliente</label>
    <input type="text" name="cliente" required>

    <label>Data</label>
    <input type="date" name="data" required>

    <div class="grid">
      <label>Colarinho <input type="number" name="colarinho" required></label>
      <label>Ombro <input type="number" name="ombro" required></label>
      <label>Tórax <input type="number" name="torax" required></label>
      <label>Cintura <input type="number" name="cintura" required></label>
      <label>Barra/Quadril <input type="number" name="quadril" required></label>
      <label>Comprimento <input type="number" name="comprimento" required></label>
      <label>Manga <input type="number" name="manga" required></label>
      <label>Bíceps <input type="number" name="biceps" required></label>
      <label>Antebraço <input type="number" name="antebraco" required></label>
      <label>Punho <input type="number" name="punho" required></label>
    </div>

    <label>Manequim</label>
    <select name="manequim" required>
      <option value="">Selecione</option>
      <option value="38">38</option>
      <option value="40">40</option>
      <option value="42">42</option>
      <option value="44">44</option>
      <option value="46">46</option>
      <option value="48">48</option>
      <option value="50">50</option>
      <option value="52">52</option>
      <option value="54">54</option>
      <option value="56">56</option>
      <option value="58">58</option>
      <option value="60">60</option>
      <option value="62">62</option>
    </select>

    <label>Tipo de colarinho</label>
    <input type="text" name="colarinho_tipo" required>

    <label>Tipo de punho</label>
    <input type="text" name="punho_tipo" required>

    <label>Modelagem</label>
    <input type="text" name="modelagem" required>

    <label>Monograma (opcional)</label>
    <input type="text" name="monograma">

    <label>Valor da camisa</label>
    <input type="text" name="valor" required>

    <label>Previsão de entrega</label>
    <input type="date" name="entrega" required>

    <label>Observações</label>
    <textarea name="obs"></textarea>

    <button type="button" onclick="gerarPDF()">Gerar PDF</button>
    <button type="submit">Imprimir</button>
  </form>

  <div id="pdfContainer" class="pdf-container"></div>

  <script>
    async function gerarPDF() {
      const form = document.getElementById('fichaForm');
      if (!form.checkValidity()) {
        alert('Preencha todos os campos obrigatórios.');
        return;
      }

      const data = new FormData(form);
      const cliente = data.get('cliente');
      const dataHoje = new Date().toLocaleDateString();
      const manequim = parseInt(data.get('manequim'));
      let metragem = '1,60m';
      if (manequim >= 50 && manequim <= 54) metragem = '1,70m';
      else if (manequim >= 56 && manequim <= 58) metragem = '1,80m';
      else if (manequim >= 60) metragem = '1,90m';

      const container = document.getElementById('pdfContainer');
      container.innerHTML = `
        <div style="font-family: Montserrat; padding: 20px; max-width: 800px;">
          <div class="logo">Di Roveri’s - Ficha Técnica</div>
          <strong>Cliente:</strong> ${cliente}<br>
          <strong>Data:</strong> ${data.get('data')}<br>
          <strong>Valor:</strong> ${data.get('valor')}<br>
          <strong>Previsão de Entrega:</strong> ${data.get('entrega')}<br>
          <strong>Manequim:</strong> ${manequim} (${metragem} de tecido)<br>
          <br>
          <strong>Medidas (cm):</strong>
          <ul>
            <li>Colarinho: ${data.get('colarinho')}</li>
            <li>Ombro: ${data.get('ombro')}</li>
            <li>Tórax: ${data.get('torax')}</li>
            <li>Cintura: ${data.get('cintura')}</li>
            <li>Quadril: ${data.get('quadril')}</li>
            <li>Comprimento: ${data.get('comprimento')}</li>
            <li>Manga: ${data.get('manga')}</li>
            <li>Bíceps: ${data.get('biceps')}</li>
            <li>Antebraço: ${data.get('antebraco')}</li>
            <li>Punho: ${data.get('punho')}</li>
          </ul>
          <strong>Colarinho:</strong> ${data.get('colarinho_tipo')}<br>
          <strong>Punho:</strong> ${data.get('punho_tipo')}<br>
          <strong>Modelagem:</strong> ${data.get('modelagem')}<br>
          <strong>Monograma:</strong> ${data.get('monograma') || 'Não informado'}<br>
          <br>
          <strong>Observações:</strong><br>${data.get('obs') || 'Nenhuma'}<br>
          <br>
          <img class="croqui" src="/mnt/data/ChatGPT Image 10 de jun. de 2025, 20_34_24.png" alt="Croqui da camisa"><br>
          <em>Documento gerado em ${dataHoje}</em>
        </div>
      `;
      container.style.display = 'block';

      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      await html2canvas(container).then(canvas => {
        const imgData = canvas.toDataURL('image/png');
        doc.addImage(imgData, 'PNG', 10, 10, 190, 0);
        doc.save(`Ficha_Tecnica_${cliente}.pdf`);
      });
    }

    document.getElementById('fichaForm').addEventListener('submit', function(e) {
      e.preventDefault();
      window.print();
    });
  </script>
</body>
</html>
