<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulação de Guerra: Radar Quântico</title>
    <style>
        body {
            margin: 0;
            padding: 20px;
            font-family: sans-serif;
            background-color: #0f172a;
            color: #f8fafc;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        h1 {
            margin-bottom: 5px;
            font-size: 24px;
            text-align: center;
        }

        p.subtitle {
            color: #94a3b8;
            margin-top: 0;
            margin-bottom: 20px;
            font-size: 14px;
        }

        .container {
            display: flex;
            gap: 20px;
            max-width: 1100px;
            width: 100%;
            flex-wrap: wrap;
            justify-content: center;
        }

        /* PAINEL LARANJA COMPATÍVEL */
        .panel {
            background-color: #ea580c;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
            width: 450px;
            box-sizing: border-box;
        }

        .panel-title {
            font-size: 14px;
            font-weight: bold;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            margin-bottom: 15px;
            color: #ffffff;
            border-bottom: 1px solid #ff7a33;
            padding-bottom: 5px;
        }

        .display-box {
            background-color: #020617;
            border-radius: 8px;
            height: 350px;
            position: relative;
            overflow: hidden;
            border: 1px solid #334155;
        }

        /* TELA DO RADAR TÁTICO */
        #radarView {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            padding: 25px;
            align-content: center;
            justify-items: center;
            box-sizing: border-box;
        }

        .controls {
            margin-top: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
            width: 100%;
        }

        .btn-group {
            display: flex;
            gap: 10px;
        }

        /* BOTÕES PRETO E VERDE */
        button {
            background-color: #000000;
            color: #10b981;
            border: 2px solid #10b981;
            padding: 12px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: bold;
            font-size: 13px;
            transition: all 0.2s;
            flex: 1;
        }

        button:hover {
            background-color: #10b981;
            color: #000000;
        }

        .slider-group {
            display: flex;
            flex-direction: column;
            gap: 5px;
            font-size: 12px;
            color: #ffffff;
            font-weight: bold;
        }

        .slider-row {
            display: flex;
            align-items: center;
            justify-content: space-between;
            gap: 10px;
        }

        input[type="range"] {
            flex: 1;
            accent-color: #000000;
        }

        .stats {
            margin-top: 10px;
            font-size: 13px;
            color: #ffffff;
            font-weight: bold;
            display: flex;
            justify-content: space-between;
        }

        /* UNIDADES MILITARES IMPERCEPTÍVEIS */
        .military-unit {
            width: 40px;
            height: 40px;
            background-color: #1e293b;
            border: 2px solid #475569;
            border-radius: 6px;
            transition: all 0.25s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 10px;
            font-weight: bold;
            color: #64748b;
        }

        /* SINAL ENCONTRADO PELO RADAR QUANTICO */
        .radar-hit {
            background-color: #22c55e !important;
            border-color: #4ade80 !important;
            color: #ffffff !important;
            box-shadow: 0 0 25px #22c55e;
            transform: scale(1.15);
        }

        /* ANOMALIA DE ASSINATURA ESCURA */
        .stealth-hit {
            background-color: #ef4444 !important;
            border-color: #f87171 !important;
            color: #ffffff !important;
            box-shadow: 0 0 25px #ef4444;
            transform: scale(1.15);
        }

        /* GRÁFICO DE ESPECTRO EM GUERRA */
        .chart-zone-top {
            position: absolute;
            left: 50px;
            top: 30px;
            width: 330px;
            height: 130px;
            background-color: rgba(34, 197, 94, 0.1);
            border-left: 2px solid #64748b;
        }

        .chart-zone-bottom {
            position: absolute;
            left: 50px;
            top: 162px;
            width: 330px;
            height: 130px;
            background-color: rgba(239, 68, 68, 0.1);
            border-left: 2px solid #64748b;
            border-bottom: 2px solid #64748b;
        }

        .chart-line {
            position: absolute;
            left: 50px;
            top: 160px;
            width: 330px;
            height: 2px;
            background-color: #ffffff;
        }

        .chart-label {
            position: absolute;
            font-size: 11px;
            font-weight: bold;
        }

        .dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            position: absolute;
            border: 2px solid #ffffff;
            transform: translate(-50%, -50%);
            animation: pop 0.3s ease-out;
        }

        @keyframes pop {
            0% { transform: translate(-50%, -50%) scale(0); }
            100% { transform: translate(-50%, -50%) scale(1); }
        }
    </style>
</head>
<body>

    <h1>Simulador de Campo de Batalha: Radar Quântico de Matéria Escura</h1>
    <p class="subtitle">Estratégias de Reconhecimento Eletrônico contra Camuflagem de Absorção Total</p>

    <div class="container">
        <!-- Painel Esquerdo: Varredura de Campo -->
        <div class="panel">
            <div class="panel-title">📡 Varredura Tática de Frequência</div>
            <div id="radarView" class="display-box">
                <!-- Setores militares gerados dinamicamente de forma segura -->
            </div>
            <div class="controls">
                <div class="btn-group">
                    <button id="scan-normal">Varredura de Radar</button>
                    <button id="scan-quantum">Pulso Quântico (Anti-Ocultação)</button>
                </div>
                <div class="slider-group">
                    <div class="slider-row">
                        <label>Potência do Pulso:</label>
                        <input type="range" id="power-slider" min="10" max="200" value="100">
                        <span id="power-val">100 MW</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Painel Direito: Análise de Assinatura -->
        <div class="panel">
            <div class="panel-title">📊 Análise de Espectro: Ruído vs Assinatura Térmica</div>
            <div id="chartView" class="display-box">
                <div class="chart-zone-top"></div>
                <div class="chart-zone-bottom"></div>
                <div class="chart-line"></div>
                
                <div class="chart-label" style="left: 200px; top: 40px; color: #4ade80;">Alvos Convencionais</div>
                <div class="chart-label" style="left: 170px; top: 250px; color: #f87171;">Assinaturas Ocultas (Invisíveis)</div>
                <div class="chart-label" style="left: 140px; top: 310px; color: #64748b;">Frequência de Retorno de Onda &rarr;</div>
            </div>
            <div class="stats">
                <span>Alvos Revelados: <strong id="count-normal" style="color:#000000">0</strong></span>
                <span>Ocultações Rompidas: <strong id="count-stealth" style="color:#ffffff">0</strong></span>
            </div>
        </div>
    </div>

    <script>
        const radarView = document.getElementById('radarView');
        const chartView = document.getElementById('chartView');
        const powerSlider = document.getElementById('power-slider');
        const powerVal = document.getElementById('power-val');

        let counters = { normal: 0, stealth: 0 };
        let currentPower = 100;

        powerSlider.addEventListener('input', function(e) {
            currentPower = parseInt(e.target.value);
            powerVal.innerText = currentPower + " MW";
        });

        // Cria a matriz de setores do mapa militar tático
        const totalSectors = 12;
        for (let i = 0; i < totalSectors; i++) {
            let unit = document.createElement('div');
            unit.className = 'military-unit';
            unit.innerText = "SET-" + (i + 1);
            radarView.appendChild(unit);
        }

        const unitList = document.querySelectorAll('.military-unit');

        // Ações de Varredura Tática
        document.getElementById('scan-normal').addEventListener('click', function() {
            triggerRadar('normal');
        });

        document.getElementById('scan-quantum').addEventListener('click', function() {
            triggerRadar('stealth');
        });

        function triggerRadar(mode) {
            let randomIndex = Math.floor(Math.random() * unitList.length);
            let selectedSector = unitList[randomIndex];

            // Executa a marcação visual dependendo do tipo de detecção quântica aplicada
            if (mode === 'normal') {
                selectedSector.classList.add('radar-hit');
                setTimeout(() => selectedSector.classList.remove('radar-hit'), 400);
            } else {
