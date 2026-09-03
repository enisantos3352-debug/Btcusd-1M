
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Matrix M1 • 6 Blocos & Médias Pro</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #0b0e11;
            color: #d1d4dc;
            margin: 0;
            padding: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        .container {
            width: 100%;
            max-width: 600px;
            background: #1e222d;
            border: 2px solid #2ecc71;
            border-radius: 12px;
            padding: 12px;
            box-sizing: border-box;
            box-shadow: 0 4px 20px rgba(46, 204, 113, 0.4);
        }
        h1 {
            color: #2ecc71;
            font-size: 15px;
            text-align: center;
            margin: 0 0 4px 0;
            text-transform: uppercase;
        }
        .sub-header {
            text-align: center;
            font-size: 9px;
            color: #848e9c;
            margin-bottom: 8px;
        }
        
        .top-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 6px;
            margin-bottom: 8px;
        }
        .box-info {
            background: #131722;
            border: 1px solid #2a2e39;
            border-radius: 6px;
            padding: 6px;
            text-align: center;
        }
        .box-label {
            font-size: 8px;
            color: #848e9c;
            text-transform: uppercase;
        }
        .live-val {
            font-size: 14px;
            font-weight: bold;
            color: #2ecc71;
            margin-top: 2px;
        }
        .timer-val {
            font-size: 12px;
            font-weight: bold;
            color: #f3ba2f;
            margin-top: 2px;
        }

        @keyframes piscar {
            0% { opacity: 0.8; }
            50% { opacity: 1; box-shadow: 0 0 12px currentColor; }
            100% { opacity: 0.8; }
        }
        .signal-box {
            background: #131722;
            border: 2px solid #2a2e39;
            border-radius: 6px;
            padding: 8px;
            text-align: center;
            margin-bottom: 8px;
            font-weight: bold;
            font-size: 11px;
            text-transform: uppercase;
            display: none;
        }
        .signal-box.alta {
            border-color: #2ecc71;
            color: #2ecc71;
            background: rgba(46, 204, 113, 0.2);
            animation: piscar 1.2s infinite;
            display: block;
        }
        .signal-box.baixa {
            border-color: #f6465d;
            color: #f6465d;
            background: rgba(246, 70, 93, 0.2);
            animation: piscar 1.2s infinite;
            display: block;
        }
        .signal-box.neutro {
            border-color: #f3ba2f;
            color: #f3ba2f;
            background: rgba(243, 186, 47, 0.1);
            display: block;
        }

        /* Painel das 6 Linhas / 3 Centros */
        .grid-linhas {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 3px;
            margin-bottom: 8px;
        }
        .linha-card {
            background: #131722;
            border: 1px solid #2a2e39;
            border-radius: 5px;
            padding: 4px 2px;
            text-align: center;
        }
        .linha-label {
            font-size: 7px;
            color: #848e9c;
            text-transform: uppercase;
        }
        .linha-val {
            font-size: 9px;
            font-weight: bold;
            color: #ffffff;
            margin-top: 2px;
        }

        .canvas-container {
            background: #131722;
            border: 1px solid #2a2e39;
            border-radius: 6px;
            padding: 4px;
        }
        canvas {
            width: 100% !important;
            height: 220px !important;
            display: block;
        }
        .footer {
            font-size: 7px;
            color: #787b86;
            text-align: center;
            margin-top: 6px;
            text-transform: uppercase;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Matrix M1 • 6 Blocos & Médias</h1>
        <div class="sub-header">M1 Comprimido • 3 Centros de Referência & MMs (19, 38, 57, 191, 385)</div>

        <div class="top-grid">
            <div class="box-info">
                <div class="box-label">Preço ao Vivo</div>
                <div class="live-val" id="preco-vivo">Carregando...</div>
            </div>
            <div class="box-info">
                <div class="box-label">Atualização M1</div>
                <div class="timer-val" id="contador-virada">--:--</div>
            </div>
        </div>

        <div class="signal-box neutro" id="caixa-sinal">
            Monitorando cruzamento dos centros...
        </div>

        <div class="grid-linhas">
            <div class="linha-card" style="border-color: #f6465d;">
                <div class="linha-label" style="color: #f6465d;">Máxima</div>
                <div class="linha-val" id="val-max">--</div>
            </div>
            <div class="linha-card" style="border-color: #3861fb;">
                <div class="linha-label" style="color: #3861fb;">C. Superior</div>
                <div class="linha-val" id="val-c-sup">--</div>
            </div>
            <div class="linha-card" style="border-color: #f3ba2f;">
                <div class="linha-label" style="color: #f3ba2f;">Centro Meio</div>
                <div class="linha-val" id="val-mid">--</div>
            </div>
            <div class="linha-card" style="border-color: #3861fb;">
                <div class="linha-label" style="color: #3861fb;">C. Inferior</div>
                <div class="linha-val" id="val-c-inf">--</div>
            </div>
            <div class="linha-card" style="border-color: #2ecc71;">
                <div class="linha-label" style="color: #2ecc71;">Mínima</div>
                <div class="linha-val" id="val-min">--</div>
            </div>
        </div>

        <div class="canvas-container">
            <canvas id="graficoBlocos"></canvas>
        </div>

        <div class="footer">Estratégia Anivaldo Pro • Operações M1</div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <script>
        let meuGrafico;

        function inicializarGrafico() {
            Chart.defaults.color = '#848e9c';
            Chart.defaults.font.size = 8;

            let ctx = document.getElementById('graficoBlocos').getContext('2d');
            meuGrafico = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: [],
                    datasets: [
                        { label: 'Preço BTC', data: [], borderColor: '#3861fb', borderWidth: 2, pointRadius: 1, tension: 0.1 },
                        { label: 'MM 19', data: [], borderColor: '#f3ba2f', borderWidth: 1.5, pointRadius: 0, tension: 0.2 },
                        { label: 'MM 38', data: [], borderColor: '#2ecc71', borderWidth: 1, pointRadius: 0, tension: 0.2 },
                        { label: 'Máxima', data: [], borderColor: '#f6465d', borderWidth: 1, pointRadius: 0, borderDash: [2, 2] },
                        { label: 'Centro Superior', data: [], borderColor: '#9b59b6', borderWidth: 1, pointRadius: 0, borderDash: [2, 2] },
                        { label: 'Centro Meio', data: [], borderColor: '#f3ba2f', borderWidth: 1, pointRadius: 0, borderDash: [2, 2] },
                        { label: 'Centro Inferior', data: [], borderColor: '#9b59b6', borderWidth: 1, pointRadius: 0, borderDash: [2, 2] },
                        { label: 'Mínima', data: [], borderColor: '#2ecc71', borderWidth: 1, pointRadius: 0, borderDash: [2, 2] }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: { display: true, grid: { color: '#1e222d' } },
                        y: { grid: { color: '#2a2e39' } }
                    },
                    plugins: {
                        legend: { display: true, position: 'bottom', labels: { boxWidth: 6, font: { size: 7 } } }
                    }
                }
            });
        }

        function calcularMM(dados, periodo) {
            let resultado = [];
            for (let i = 0; i < dados.length; i++) {
                if (i < periodo - 1) {
                    resultado.push(dados[i]);
                } else {
                    let slice = dados.slice(i - periodo + 1, i + 1);
                    let soma = slice.reduce((a, b) => a + b, 0);
                    resultado.push(soma / periodo);
                }
            }
            return resultado;
        }

        async function atualizarPainel() {
            try {
                let resposta = await fetch('https://api.binance.com/api/v3/klines?symbol=BTCUSDT&interval=1m&limit=50');
                let dados = await resposta.json();

                let fechamentos = dados.map(d => parseFloat(d[4]));
                let maximasVelas = dados.map(d => parseFloat(d[2]));
                let minimasVelas = dados.map(d => parseFloat(d[3]));
                let horarios = dados.map(d => new Date(d[0]).toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'}));

                let precoAtual = fechamentos[fechamentos.length - 1];
                document.getElementById('preco-vivo').innerText = '$ ' + precoAtual.toLocaleString('en-US', {minimumFractionDigits: 2});

                let maximaDiaria = Math.max(...maximasVelas);
                let minimaDiaria = Math.min(...minimasVelas);
                
                let centroMeio = (maximaDiaria + minimaDiaria) / 2;
                let centroSuperior = (maximaDiaria + centroMeio) / 2;
                let centroInferior = (centroMeio + minimaDiaria) / 2;

                document.getElementById('val-max').innerText = '$ ' + maximaDiaria.toFixed(0);
                document.getElementById('val-c-sup').innerText = '$ ' + centroSuperior.toFixed(0);
                document.getElementById('val-mid').innerText = '$ ' + centroMeio.toFixed(0);
                document.getElementById('val-c-inf').innerText = '$ ' + centroInferior.toFixed(0);
                document.getElementById('val-min').innerText = '$ ' + minimaDiaria.toFixed(0);

                let mm19 = calcularMM(fechamentos, 19);
                let mm38 = calcularMM(fechamentos, 38);
                let ultimaMM19 = mm19[mm19.length - 1];

                meuGrafico.data.labels = horarios;
                meuGrafico.data.datasets[0].data = fechamentos;
                meuGrafico.data.datasets[1].data = mm19;
                meuGrafico.data.datasets[2].data = mm38;
                meuGrafico.data.datasets[3].data = new Array(fechamentos.length).fill(maximaDiaria);
                meuGrafico.data.datasets[4].data = new Array(fechamentos.length).fill(centroSuperior);
                meuGrafico.data.datasets[5].data = new Array(fechamentos.length).fill(centroMeio);
                meuGrafico.data.datasets[6].data = new Array(fechamentos.length).fill(centroInferior);
                meuGrafico.data.datasets[7].data = new Array(fechamentos.length).fill(minimaDiaria);
                meuGrafico.update();

                let caixaSinal = document.getElementById('caixa-sinal');
                if (precoAtual > centroMeio && ultimaMM19 > centroMeio) {
                    caixaSinal.className = "signal-box alta";
                    caixaSinal.innerText = `🚀 SINAL DE COMPRA • ACIMA DO CENTRO (RUMO À MÁXIMA)`;
                } else if (precoAtual < centroMeio && ultimaMM19 < centroMeio) {
                    caixaSinal.className = "signal-box baixa";
                    caixaSinal.innerText = `⚠️ SINAL DE VENDA • ABAIXO DO CENTRO (RUMO À MÍNIMA)`;
                } else {
                    caixaSinal.className = "signal-box neutro";
                    caixaSinal.innerText = `⚖️ TRABALHANDO NO CENTRO DE REFERÊNCIA`;
                }

            } catch (e) {
                console.log("Erro:", e);
            }
        }

        function rodarContador() {
            let agora = new Date();
            let seg = agora.getSeconds();
            let segRestantes = 59 - seg;
            document.getElementById('contador-virada').innerText = '00:' + (segRestantes < 10 ? '0' + segRestantes : segRestantes);
        }

        inicializarGrafico();
        atualizarPainel();
        rodarContador();
        setInterval(atualizarPainel, 5000);
        setInterval(rodarContador, 1000);
    </script>

</body>
</html>
