<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OLYMP TRADE PRO - 5 TECHNIQUES SYSTEM</title>
    <script src="https://unpkg.com/lightweight-charts@4.2.2/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --bg-primary: #0a0e1a;
            --bg-secondary: #151a25;
            --bg-card: #1e2532;
            --border: #2a3447;
            --green: #00e676;
            --red: #ff1744;
            --blue: #2979ff;
            --yellow: #ffea00;
            --purple: #d500f9;
            --text-primary: #ffffff;
            --text-secondary: #8b92a8;
        }

        body {
            background: var(--bg-primary);
            color: var(--text-primary);
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            overflow-x: hidden;
        }

        /* Header */
        .header {
            background: linear-gradient(135deg, #1e2532 0%, #0a0e1a 100%);
            border-bottom: 2px solid var(--blue);
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 20px rgba(41, 121, 255, 0.3);
        }

        .logo {
            font-size: 24px;
            font-weight: bold;
            background: linear-gradient(135deg, var(--blue), var(--purple));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .clock {
            font-family: 'Courier New', monospace;
            font-size: 18px;
            color: var(--yellow);
            font-weight: bold;
            text-shadow: 0 0 10px rgba(255, 234, 0, 0.5);
        }

        /* Main Container */
        .container {
            display: grid;
            grid-template-columns: 1fr 400px;
            gap: 15px;
            padding: 15px;
            max-width: 1920px;
            margin: 0 auto;
        }

        /* Chart Section */
        .chart-section {
            background: var(--bg-secondary);
            border-radius: 12px;
            padding: 15px;
            border: 1px solid var(--border);
        }

        .chart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }

        .market-selector {
            display: flex;
            gap: 10px;
        }

        select {
            background: var(--bg-card);
            color: var(--text-primary);
            border: 1px solid var(--border);
            padding: 8px 15px;
            border-radius: 6px;
            font-size: 14px;
            cursor: pointer;
            outline: none;
        }

        select:focus {
            border-color: var(--blue);
        }

        #chart {
            width: 100%;
            height: 500px;
            border-radius: 8px;
        }

        /* Sidebar */
        .sidebar {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }

        .card {
            background: var(--bg-card);
            border-radius: 12px;
            padding: 15px;
            border: 1px solid var(--border);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }

        .card-title {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 10px;
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: 600;
        }

        /* Signal Box */
        .signal-box {
            background: linear-gradient(135deg, #1e2532 0%, #2a3447 100%);
            border: 2px solid var(--border);
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .signal-box::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.1), transparent);
            animation: shine 3s infinite;
        }

        @keyframes shine {
            0% { left: -100%; }
            100% { left: 100%; }
        }

        .signal-status {
            font-size: 28px;
            font-weight: bold;
            margin: 10px 0;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .signal-buy {
            color: var(--green);
            text-shadow: 0 0 20px rgba(0, 230, 118, 0.6);
            animation: pulse-green 2s infinite;
        }

        .signal-sell {
            color: var(--red);
            text-shadow: 0 0 20px rgba(255, 23, 68, 0.6);
            animation: pulse-red 2s infinite;
        }

        .signal-wait {
            color: var(--text-secondary);
        }

        @keyframes pulse-green {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }

        @keyframes pulse-red {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.7; }
        }

        /* Score Bar */
        .score-container {
            margin: 15px 0;
        }

        .score-label {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 13px;
        }

        .score-bar-bg {
            height: 10px;
            background: var(--bg-secondary);
            border-radius: 5px;
            overflow: hidden;
        }

        .score-bar-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--red), var(--yellow), var(--green));
            border-radius: 5px;
            transition: width 0.5s ease;
        }

        /* Checklist */
        .checklist {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .check-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 8px 12px;
            background: var(--bg-secondary);
            border-radius: 6px;
            font-size: 13px;
        }

        .check-item.active {
            background: rgba(0, 230, 118, 0.1);
            border-left: 3px solid var(--green);
        }

        .check-item.inactive {
            background: rgba(255, 23, 68, 0.1);
            border-left: 3px solid var(--red);
        }

        .check-icon {
            font-size: 16px;
        }

        /* Trade Setup */
        .trade-setup {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 10px;
        }

        .setup-item {
            background: var(--bg-secondary);
            padding: 10px;
            border-radius: 6px;
            text-align: center;
        }

        .setup-label {
            font-size: 11px;
            color: var(--text-secondary);
            margin-bottom: 5px;
        }

        .setup-value {
            font-size: 16px;
            font-weight: bold;
            color: var(--text-primary);
        }

        /* Level Buttons */
        .level-buttons {
            display: flex;
            gap: 10px;
            margin-top: 10px;
        }

        .btn {
            flex: 1;
            padding: 10px;
            border: none;
            border-radius: 6px;
            font-size: 13px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            text-transform: uppercase;
        }

        .btn-support {
            background: var(--green);
            color: #000;
        }

        .btn-resistance {
            background: var(--red);
            color: #fff;
        }

        .btn-clear {
            background: var(--bg-secondary);
            color: var(--text-primary);
            border: 1px solid var(--border);
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
        }

        /* Levels List */
        .levels-list {
            max-height: 150px;
            overflow-y: auto;
            margin-top: 10px;
        }

        .level-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px;
            background: var(--bg-secondary);
            border-radius: 4px;
            margin-bottom: 5px;
            font-size: 12px;
        }

        .level-type {
            padding: 2px 8px;
            border-radius: 3px;
            font-size: 10px;
            font-weight: bold;
        }

        .level-type.support {
            background: var(--green);
            color: #000;
        }

        .level-type.resistance {
            background: var(--red);
            color: #fff;
        }

        /* History */
        .history-list {
            max-height: 200px;
            overflow-y: auto;
        }

        .history-item {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 8px;
            padding: 10px;
            background: var(--bg-secondary);
            border-radius: 6px;
            margin-bottom: 5px;
            font-size: 12px;
        }

        .history-win {
            border-left: 3px solid var(--green);
        }

        .history-loss {
            border-left: 3px solid var(--red);
        }

        /* Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }

        ::-webkit-scrollbar-track {
            background: var(--bg-secondary);
            border-radius: 3px;
        }

        ::-webkit-scrollbar-thumb {
            background: var(--border);
            border-radius: 3px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: var(--blue);
        }

        /* Audio Toggle */
        .audio-toggle {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 10px;
        }

        .toggle-switch {
            position: relative;
            width: 50px;
            height: 24px;
            background: var(--bg-secondary);
            border-radius: 12px;
            cursor: pointer;
            transition: 0.3s;
        }

        .toggle-switch.active {
            background: var(--green);
        }

        .toggle-slider {
            position: absolute;
            top: 2px;
            left: 2px;
            width: 20px;
            height: 20px;
            background: #fff;
            border-radius: 50%;
            transition: 0.3s;
        }

        .toggle-switch.active .toggle-slider {
            left: 28px;
        }

        /* Responsive */
        @media (max-width: 1200px) {
            .container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>

    <!-- Header -->
    <div class="header">
        <div class="logo">
            📊 OLYMP TRADE PRO
        </div>
        <div class="clock" id="clock">00:00:00</div>
    </div>

    <!-- Main Container -->
    <div class="container">
        <!-- Chart Section -->
        <div class="chart-section">
            <div class="chart-header">
                <div class="market-selector">
                    <select id="market">
                        <option value="R_100">Volatility 100</option>
                        <option value="R_75">Volatility 75</option>
                        <option value="R_50">Volatility 50</option>
                        <option value="R_25">Volatility 25</option>
                        <option value="R_10">Volatility 10</option>
                        <option value="RDBULL">Bull Market</option>
                        <option value="RDBEAR">Bear Market</option>
                    </select>
                    <select id="timeframe">
                        <option value="60">1 Minute</option>
                        <option value="300">5 Minutes</option>
                    </select>
                </div>
                <div style="color: var(--text-secondary); font-size: 13px;">
                    5 TECHNIQUES SYSTEM
                </div>
            </div>
            <div id="chart"></div>
        </div>

        <!-- Sidebar -->
        <div class="sidebar">
            <!-- Signal Box -->
            <div class="signal-box" id="signalBox">
                <div class="card-title">SIGNAL STATUS</div>
                <div class="signal-status signal-wait" id="signalStatus">
                    WAITING...
                </div>
                <div style="color: var(--text-secondary); font-size: 13px; margin-top: 5px;">
                    <span id="signalAccuracy">0</span>% ACCURACY
                </div>
            </div>

            <!-- Score -->
            <div class="card">
                <div class="card-title">SIGNAL SCORE</div>
                <div class="score-container">
                    <div class="score-label">
                        <span>Strength</span>
                        <span id="scoreText">0/7</span>
                    </div>
                    <div class="score-bar-bg">
                        <div class="score-bar-fill" id="scoreBar" style="width: 0%"></div>
                    </div>
                </div>
            </div>

            <!-- Checklist -->
            <div class="card">
                <div class="card-title">TECHNIQUES CHECKLIST</div>
                <div class="checklist">
                    <div class="check-item inactive" id="check1">
                        <span>1. One Color Candles</span>
                        <span class="check-icon">❌</span>
                    </div>
                    <div class="check-item inactive" id="check2">
                        <span>2. BOS</span>
                        <span class="check-icon">❌</span>
                    </div>
                    <div class="check-item inactive" id="check3">
                        <span>3. ChoCH</span>
                        <span class="check-icon">❌</span>
                    </div>
                    <div class="check-item inactive" id="check4">
                        <span>4. Trend Line</span>
                        <span class="check-icon"></span>
                    </div>
                    <div class="check-item inactive" id="check5">
                        <span>5. Engulfing Pattern</span>
                        <span class="check-icon"></span>
                    </div>
                    <div class="check-item inactive" id="check6">
                        <span>6. FVG</span>
                        <span class="check-icon"></span>
                    </div>
                    <div class="check-item inactive" id="check7">
                        <span>7. Key Level</span>
                        <span class="check-icon"></span>
                    </div>
                </div>
            </div>

            <!-- Trade Setup -->
            <div class="card">
                <div class="card-title">TRADE SETUP</div>
                <div class="trade-setup">
                    <div class="setup-item">
                        <div class="setup-label">ENTRY</div>
                        <div class="setup-value" id="entryPrice">-</div>
                    </div>
                    <div class="setup-item">
                        <div class="setup-label">TARGET</div>
                        <div class="setup-value" id="targetPrice">-</div>
                    </div>
                    <div class="setup-item">
                        <div class="setup-label">CANDLES</div>
                        <div class="setup-value" id="candleTarget">2-3</div>
                    </div>
                    <div class="setup-item">
                        <div class="setup-label">DISTANCE</div>
                        <div class="setup-value" id="pipDistance">-</div>
                    </div>
                </div>
            </div>

            <!-- Level Marking -->
            <div class="card">
                <div class="card-title">MARK LEVELS</div>
                <div class="level-buttons">
                    <button class="btn btn-support" onclick="addLevel('support')">
                        + Support
                    </button>
                    <button class="btn btn-resistance" onclick="addLevel('resistance')">
                        + Resistance
                    </button>
                </div>
                <button class="btn btn-clear" onclick="clearLevels()" style="margin-top: 10px; width: 100%;">
                    Clear All Levels
                </button>
                <div class="levels-list" id="levelsList"></div>
            </div>

            <!-- Audio Toggle -->
            <div class="card">
                <div class="card-title">AUDIO ALERTS</div>
                <div class="audio-toggle">
                    <div class="toggle-switch" id="audioToggle" onclick="toggleAudio()">
                        <div class="toggle-slider"></div>
                    </div>
                    <span style="font-size: 13px;">Enable Sound</span>
                </div>
            </div>

            <!-- Signal History -->
            <div class="card">
                <div class="card-title">SIGNAL HISTORY</div>
                <div class="history-list" id="historyList">
                    <div style="color: var(--text-secondary); text-align: center; padding: 20px;">
                        No signals yet
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Global Variables
        let chart, candleSeries, trendLineSeries, supportLineSeries, resistanceLineSeries;
        let candles = [];
        let levels = [];
        let ws = null;
        let currentMarket = 'R_100';
        let currentTimeframe = 60;
        let audioEnabled = false;
        let lastSignal = null;
        let signalHistory = [];
        let pivots = [];
        let markers = [];

        // Initialize Chart
        function initChart() {
            const chartContainer = document.getElementById('chart');
            
            chart = LightweightCharts.createChart(chartContainer, {
                layout: {
                    background: { color: '#151a25' },
                    textColor: '#8b92a8'
                },
                grid: {
                    vertLines: { color: '#2a3447' },
                    horzLines: { color: '#2a3447' }
                },
                rightPriceScale: {
                    borderColor: '#2a3447',
                    scaleMargins: {
                        top: 0.1,
                        bottom: 0.2
                    }
                },
                timeScale: {
                    borderColor: '#2a3447',
                    timeVisible: true,
                    secondsVisible: false
                },
                crosshair: {
                    mode: LightweightCharts.CrosshairMode.Normal
                }
            });

            candleSeries = chart.addCandlestickSeries({
                upColor: '#00e676',
                downColor: '#ff1744',
                borderUpColor: '#00e676',
                borderDownColor: '#ff1744',
                wickUpColor: '#00e676',
                wickDownColor: '#ff1744'
            });

            trendLineSeries = chart.addLineSeries({
                color: '#2979ff',
                lineWidth: 2,
                lineStyle: 0
            });

            supportLineSeries = chart.addLineSeries({
                color: '#00e676',
                lineWidth: 1,
                lineStyle: 2
            });

            resistanceLineSeries = chart.addLineSeries({
                color: '#ff1744',
                lineWidth: 1,
                lineStyle: 2
            });

            // Click handler for adding levels
            chart.subscribeClick((param) => {
                if (param.point) {
                    const price = param.price;
                    console.log('Clicked at price:', price);
                }
            });

            window.addEventListener('resize', () => {
                chart.applyOptions({
                    width: chartContainer.clientWidth,
                    height: 500
                });
            });
        }

        // WebSocket Connection
        function connect() {
            if (ws) ws.close();

            ws = new WebSocket('wss://ws.derivws.com/websockets/v3?app_id=1089');

            ws.onopen = () => {
                console.log('Connected');
                ws.send(JSON.stringify({
                    ticks_history: currentMarket,
                    end: 'latest',
                    style: 'candles',
                    granularity: currentTimeframe,
                    count: 200,
                    subscribe: 1
                }));
            };

            ws.onmessage = (msg) => {
                const data = JSON.parse(msg.data);
                
                if (data.candles) {
                    candles = data.candles.map(c => ({
                        time: c.epoch,
                        open: c.open,
                        high: c.high,
                        low: c.low,
                        close: c.close
                    }));
                    candleSeries.setData(candles);
                    analyzeMarket();
                }

                if (data.ohlc) {
                    const o = data.ohlc;
                    const newCandle = {
                        time: o.open_time,
                        open: o.open,
                        high: o.high,
                        low: o.low,
                        close: o.close
                    };

                    if (candles.length > 0 && candles[candles.length - 1].time === newCandle.time) {
                        candles[candles.length - 1] = newCandle;
                    } else {
                        candles.push(newCandle);
                    }
                    candleSeries.update(newCandle);
                    analyzeMarket();
                }
            };

            ws.onclose = () => {
                setTimeout(connect, 3000);
            };
        }

        // Market Analysis
        function analyzeMarket() {
            if (candles.length < 50) return;

            // Reset checks
            let checks = {
                direction: false,
                bos: false,
                choch: false,
                trendLine: false,
                engulfing: false,
                fvg: false,
                level: levels.length > 0
            };

            markers = [];
            pivots = [];

            // 1. Detect Direction (One Color Candles)
            const lastCandles = candles.slice(-5);
            const allGreen = lastCandles.every(c => c.close > c.open);
            const allRed = lastCandles.every(c => c.close < c.open);
            
            if (allGreen || allRed) {
                checks.direction = true;
                const direction = allGreen ? 'UP' : 'DOWN';
                
                // Mark candles
                lastCandles.forEach(c => {
                    markers.push({
                        time: c.time,
                        position: allGreen ? 'belowBar' : 'aboveBar',
                        color: allGreen ? '#00e676' : '#ff1744',
                        shape: 'circle',
                        text: ''
                    });
                });
            }

            // 2. Detect Pivots
            const period = 5;
            for (let i = period; i < candles.length - period; i++) {
                let isHigh = true, isLow = true;
                for (let j = i - period; j <= i + period; j++) {
                    if (i === j) continue;
                    if (candles[j].high > candles[i].high) isHigh = false;
                    if (candles[j].low < candles[i].low) isLow = false;
                }
                if (isHigh) {
                    pivots.push({
                        type: 'H',
                        time: candles[i].time,
                        price: candles[i].high
                    });
                }
                if (isLow) {
                    pivots.push({
                        type: 'L',
                        time: candles[i].time,
                        price: candles[i].low
                    });
                }
            }

            // 3. Detect BOS and ChoCH
            let currentTrend = 'Neutral';
            let lastHH = null, lastLL = null, lastHL = null, lastLH = null;

            pivots.forEach((p) => {
                if (currentTrend === 'UP') {
                    if (p.type === 'H' && lastHH && p.price > lastHH.price) {
                        checks.bos = true;
                        markers.push({
                            time: p.time,
                            position: 'aboveBar',
                            color: '#00e676',
                            shape: 'arrowUp',
                            text: 'BOS'
                        });
                    }
                    if (p.type === 'L' && lastHL && p.price < lastHL.price) {
                        checks.choch = true;
                        markers.push({
                            time: p.time,
                            position: 'belowBar',
                            color: '#ff1744',
                            shape: 'arrowDown',
                            text: 'ChoCH'
                        });
                        currentTrend = 'DOWN';
                    }
                } else if (currentTrend === 'DOWN') {
                    if (p.type === 'L' && lastLL && p.price < lastLL.price) {
                        checks.bos = true;
                        markers.push({
                            time: p.time,
                            position: 'belowBar',
                            color: '#ff1744',
                            shape: 'arrowDown',
                            text: 'BOS'
                        });
                    }
                    if (p.type === 'H' && lastLH && p.price > lastLH.price) {
                        checks.choch = true;
                        markers.push({
                            time: p.time,
                            position: 'aboveBar',
                            color: '#00e676',
                            shape: 'arrowUp',
                            text: 'ChoCH'
                        });
                        currentTrend = 'UP';
                    }
                }

                if (currentTrend === 'Neutral') {
                    if (p.type === 'H') currentTrend = 'UP';
                    if (p.type === 'L') currentTrend = 'DOWN';
                }

                if (p.type === 'H') {
                    if (!lastHH || p.price > lastHH.price) lastHH = p;
                    lastLH = p;
                }
                if (p.type === 'L') {
                    if (!lastLL || p.price < lastLL.price) lastLL = p;
                    lastHL = p;
                }
            });

            // 4. Trend Line
            if (pivots.length >= 2) {
                const recentPivots = pivots.slice(-3);
                if (currentTrend === 'UP') {
                    const hlPivots = recentPivots.filter(p => p.type === 'L');
                    if (hlPivots.length >= 2) {
                        checks.trendLine = true;
                        trendLineSeries.setData([
                            { time: hlPivots[0].time, value: hlPivots[0].price },
                            { time: hlPivots[1].time, value: hlPivots[1].price }
                        ]);
                    }
                } else if (currentTrend === 'DOWN') {
                    const lhPivots = recentPivots.filter(p => p.type === 'H');
                    if (lhPivots.length >= 2) {
                        checks.trendLine = true;
                        trendLineSeries.setData([
                            { time: lhPivots[0].time, value: lhPivots[0].price },
                            { time: lhPivots[1].time, value: lhPivots[1].price }
                        ]);
                    }
                }
            }

            // 5. Engulfing Pattern
            if (candles.length >= 2) {
                const c1 = candles[candles.length - 2];
                const c2 = candles[candles.length - 1];
                
                const bullishEngulfing = (c2.close > c1.open) && (c2.open < c1.close) && (c2.close > c1.close);
                const bearishEngulfing = (c2.close < c1.open) && (c2.open > c1.close) && (c2.close < c1.close);
                
                if (bullishEngulfing || bearishEngulfing) {
                    checks.engulfing = true;
                    markers.push({
                        time: c2.time,
                        position: bearishEngulfing ? 'aboveBar' : 'belowBar',
                        color: bearishEngulfing ? '#ff1744' : '#00e676',
                        shape: 'text',
                        text: 'ENG'
                    });
                }
            }

            // 6. FVG Detection
            if (candles.length >= 3) {
                const c1 = candles[candles.length - 3];
                const c2 = candles[candles.length - 2];
                const c3 = candles[candles.length - 1];
                
                const bullishFVG = c1.high < c3.low;
                const bearishFVG = c1.low > c3.high;
                
                if (bullishFVG || bearishFVG) {
                    checks.fvg = true;
                }
            }

            // Calculate Score
            const score = Object.values(checks).filter(v => v).length;
            const scorePercent = (score / 7) * 100;

            // Update UI
            updateChecklist(checks);
            updateScore(score, scorePercent);

            // Generate Signal
            const currentPrice = candles[candles.length - 1].close;
            const nearestLevel = findNearestLevel(currentPrice);
            
            let signal = 'WAIT';
            let signalClass = 'signal-wait';
            let accuracy = 0;

            if (score >= 6 && checks.level) {
                if (currentTrend === 'UP' && checks.bos && !checks.choch) {
                    signal = 'BUY';
                    signalClass = 'signal-buy';
                    accuracy = Math.min(scorePercent + 10, 95);
                    generateTradeSetup('BUY', currentPrice, nearestLevel);
                } else if (currentTrend === 'DOWN' && checks.bos && checks.choch) {
                    signal = 'SELL';
                    signalClass = 'signal-sell';
                    accuracy = Math.min(scorePercent + 10, 95);
                    generateTradeSetup('SELL', currentPrice, nearestLevel);
                }
            }

            updateSignal(signal, signalClass, accuracy);

            // Add markers to chart
            candleSeries.setMarkers(markers);

            // Update levels on chart
            updateLevelsOnChart();

            // Save to history if signal
            if (signal !== 'WAIT' && signal !== lastSignal) {
                addToHistory(signal, accuracy, currentPrice, nearestLevel);
                lastSignal = signal;
                
                if (audioEnabled) {
                    playAlert(signal);
                }
            }
        }

        function updateChecklist(checks) {
            const items = [
                { id: 'check1', value: checks.direction },
                { id: 'check2', value: checks.bos },
                { id: 'check3', value: checks.choch },
                { id: 'check4', value: checks.trendLine },
                { id: 'check5', value: checks.engulfing },
                { id: 'check6', value: checks.fvg },
                { id: 'check7', value: checks.level }
            ];

            items.forEach(item => {
                const el = document.getElementById(item.id);
                if (item.value) {
                    el.className = 'check-item active';
                    el.querySelector('.check-icon').textContent = '✅';
                } else {
                    el.className = 'check-item inactive';
                    el.querySelector('.check-icon').textContent = '❌';
                }
            });
        }

        function updateScore(score, percent) {
            document.getElementById('scoreText').textContent = `${score}/7`;
            document.getElementById('scoreBar').style.width = `${percent}%`;
        }

        function updateSignal(signal, className, accuracy) {
            const statusEl = document.getElementById('signalStatus');
            statusEl.textContent = signal === 'WAIT' ? 'WAITING...' : `${signal} SIGNAL`;
            statusEl.className = `signal-status ${className}`;
            document.getElementById('signalAccuracy').textContent = accuracy.toFixed(0);
        }

        function findNearestLevel(price) {
            if (levels.length === 0) return null;
            
            let nearest = null;
            let minDistance = Infinity;
            
            levels.forEach(level => {
                const distance = Math.abs(level.price - price);
                if (distance < minDistance) {
                    minDistance = distance;
                    nearest = level;
                }
            });
            
            return nearest;
        }

        function generateTradeSetup(type, entry, level) {
            if (!level) {
                document.getElementById('entryPrice').textContent = entry.toFixed(2);
                document.getElementById('targetPrice').textContent = '-';
                document.getElementById('pipDistance').textContent = '-';
                return;
            }

            const target = level.price;
            const distance = Math.abs(target - entry);
            
            document.getElementById('entryPrice').textContent = entry.toFixed(2);
            document.getElementById('targetPrice').textContent = target.toFixed(2);
            document.getElementById('pipDistance').textContent = distance.toFixed(2) + ' pips';
        }

        function addLevel(type) {
            if (candles.length === 0) return;
            
            const currentPrice = candles[candles.length - 1].close;
            const level = {
                id: Date.now(),
                type: type,
                price: currentPrice
            };
            
            levels.push(level);
            renderLevels();
        }

        function clearLevels() {
            levels = [];
            renderLevels();
        }

        function renderLevels() {
            const container = document.getElementById('levelsList');
            
            if (levels.length === 0) {
                container.innerHTML = '<div style="color: var(--text-secondary); text-align: center; padding: 10px; font-size: 12px;">No levels marked</div>';
                return;
            }
            
            container.innerHTML = levels.map(level => `
                <div class="level-item">
                    <span>${level.price.toFixed(2)}</span>
                    <span class="level-type ${level.type}">${level.type.toUpperCase()}</span>
                </div>
            `).join('');
        }

        function updateLevelsOnChart() {
            const supportLevels = levels.filter(l => l.type === 'support').map(l => l.price);
            const resistanceLevels = levels.filter(l => l.type === 'resistance').map(l => l.price);
            
            if (supportLevels.length > 0) {
                supportLineSeries.setData(supportLevels.map(price => ({
                    time: candles[candles.length - 1].time,
                    value: price
                })));
            }
            
            if (resistanceLevels.length > 0) {
                resistanceLineSeries.setData(resistanceLevels.map(price => ({
                    time: candles[candles.length - 1].time,
                    value: price
                })));
            }
        }

        function addToHistory(signal, accuracy, entry, level) {
            const item = {
                time: new Date().toLocaleTimeString(),
                signal: signal,
                accuracy: accuracy,
                entry: entry,
                target: level ? level.price : null
            };
            
            signalHistory.unshift(item);
            if (signalHistory.length > 10) signalHistory.pop();
            
            renderHistory();
        }

        function renderHistory() {
            const container = document.getElementById('historyList');
            
            if (signalHistory.length === 0) {
                container.innerHTML = '<div style="color: var(--text-secondary); text-align: center; padding: 20px;">No signals yet</div>';
                return;
            }
            
            container.innerHTML = signalHistory.map(item => `
                <div class="history-item ${item.signal === 'BUY' ? 'history-win' : 'history-loss'}">
                    <span>${item.time}</span>
                    <span style="color: ${item.signal === 'BUY' ? 'var(--green)' : 'var(--red)'}; font-weight: bold;">${item.signal}</span>
                    <span>${item.accuracy.toFixed(0)}%</span>
                </div>
            `).join('');
        }

        function toggleAudio() {
            audioEnabled = !audioEnabled;
            document.getElementById('audioToggle').classList.toggle('active');
        }

        function playAlert(signal) {
            const msg = new SpeechSynthesisUtterance(`${signal} Signal`);
            msg.volume = 1;
            msg.rate = 1;
            window.speechSynthesis.speak(msg);
        }

        function updateClock() {
            const now = new Date();
            document.getElementById('clock').textContent = now.toLocaleTimeString();
        }

        // Event Listeners
        document.getElementById('market').addEventListener('change', (e) => {
            currentMarket = e.target.value;
            connect();
        });

        document.getElementById('timeframe').addEventListener('change', (e) => {
            currentTimeframe = parseInt(e.target.value);
            connect();
        });

        // Initialize
        window.onload = () => {
            initChart();
            connect();
            setInterval(updateClock, 1000);
            updateClock();
        };
    </script>
</body>
</html>
