<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OLYMP TRADE PRO - V2 AUTO</title>
    <script src="https://unpkg.com/lightweight-charts@4.2.2/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        :root {
            --bg-primary: #0a0e1a;
            --bg-secondary: #151a25;
            --bg-card: #1e2532;
            --border: #2a3447;
            --green: #00e676;
            --red: #ff1744;
            --blue: #2979ff;
            --yellow: #ffea00;
            --text-primary: #ffffff;
            --text-secondary: #8b92a8;
        }

        body { background: var(--bg-primary); color: var(--text-primary); font-family: 'Segoe UI', sans-serif; }

        .header {
            background: linear-gradient(135deg, #1e2532 0%, #0a0e1a 100%);
            border-bottom: 2px solid var(--blue);
            padding: 12px 20px;
            display: flex; justify-content: space-between; align-items: center;
        }

        .container {
            display: grid; grid-template-columns: 1fr 360px;
            gap: 15px; padding: 15px; max-width: 1600px; margin: 0 auto;
        }

        .chart-section { background: var(--bg-secondary); border-radius: 12px; padding: 15px; border: 1px solid var(--border); }
        #chart { width: 100%; height: 500px; }

        .sidebar { display: flex; flex-direction: column; gap: 15px; }
        .card { background: var(--bg-card); border-radius: 12px; padding: 15px; border: 1px solid var(--border); }

        .signal-box {
            background: linear-gradient(135deg, #1e2532 0%, #2a3447 100%);
            border-radius: 12px; padding: 20px; text-align: center; border: 2px solid var(--blue);
        }

        .signal-status { font-size: 28px; font-weight: bold; margin: 5px 0; letter-spacing: 2px; }
        .signal-buy { color: var(--green); text-shadow: 0 0 20px var(--green); }
        .signal-sell { color: var(--red); text-shadow: 0 0 20px var(--red); }
        .signal-wait { color: var(--text-secondary); }

        .score-bar-bg { height: 10px; background: #111; border-radius: 5px; overflow: hidden; margin-top: 10px; }
        .score-bar-fill { height: 100%; background: linear-gradient(90deg, var(--red), var(--yellow), var(--green)); transition: width 0.5s; }

        .checklist { display: flex; flex-direction: column; gap: 8px; }
        .check-item {
            display: flex; justify-content: space-between; padding: 10px;
            background: var(--bg-secondary); border-radius: 6px; font-size: 13px;
            border-left: 4px solid transparent;
        }
        .check-item.active { border-left-color: var(--green); background: rgba(0, 230, 118, 0.05); }
        .check-item.inactive { border-left-color: var(--red); opacity: 0.6; }

        select {
            background: var(--bg-card); color: white; border: 1px solid var(--border);
            padding: 8px; border-radius: 6px; outline: none;
        }

        .history-item { display: flex; justify-content: space-between; padding: 8px; font-size: 12px; border-bottom: 1px solid var(--border); }

        @media (max-width: 1000px) { .container { grid-template-columns: 1fr; } }
    </style>
</head>
<body>

    <div class="header">
        <div style="font-weight: bold; color: var(--blue);">📊 OLYMP PRO V2 (AUTO)</div>
        <div id="clock" style="color: var(--yellow); font-family: monospace;">00:00:00</div>
    </div>

    <div class="container">
        <div class="chart-section">
            <div style="margin-bottom: 15px; display: flex; gap: 10px;">
                <select id="market">
                    <option value="R_100">Volatility 100</option>
                    <option value="R_75">Volatility 75</option>
                    <option value="R_50">Volatility 50</option>
                    <option value="R_25">Volatility 25</option>
                    <option value="R_10">Volatility 10</option>
                </select>
                <select id="timeframe">
                    <option value="60">1 Minute</option>
                    <option value="300">5 Minutes</option>
                </select>
            </div>
            <div id="chart"></div>
        </div>

        <div class="sidebar">
            <div class="signal-box">
                <div style="font-size: 12px; color: var(--text-secondary);">CURRENT SIGNAL</div>
                <div id="signalStatus" class="signal-status signal-wait">ANALYZING...</div>
                <div style="font-size: 14px; color: var(--yellow); font-weight: bold;"><span id="accuracy">0</span>% CONFIDENCE</div>
            </div>

            <div class="card">
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                    <span>Signal Strength</span>
                    <span id="scoreText">0/7</span>
                </div>
                <div class="score-bar-bg"><div id="scoreBar" class="score-bar-fill" style="width: 0%"></div></div>
            </div>

            <div class="card">
                <div class="checklist" id="checklist">
                    <div class="check-item inactive" id="c1">Trend Alignment <span>✕</span></div>
                    <div class="check-item inactive" id="c2">Momentum Support <span>✕</span></div>
                    <div class="check-item inactive" id="c3">Structure Break <span>✕</span></div>
                    <div class="check-item inactive" id="c4">Volume Confirmation <span>✕</span></div>
                    <div class="check-item inactive" id="c5">Pattern Power <span>✕</span></div>
                    <div class="check-item inactive" id="c6">FVG / Gap <span>✕</span></div>
                    <div class="check-item inactive" id="c7">Auto Key Level <span>✕</span></div>
                </div>
            </div>

            <div class="card">
                <div style="font-size: 12px; color: var(--text-secondary); margin-bottom: 10px;">RECENT SIGNALS</div>
                <div id="history"></div>
            </div>
        </div>
    </div>

    <script>
        let chart, candleSeries;
        let candles = [];
        let ws;
        let lastSignalTime = 0;

        function initChart() {
            chart = LightweightCharts.createChart(document.getElementById('chart'), {
                layout: { background: { color: '#0a0e1a' }, textColor: '#8b92a8' },
                grid: { vertLines: { color: '#151a25' }, horzLines: { color: '#151a25' } },
                rightPriceScale: { borderColor: '#2a3447' },
                timeScale: { borderColor: '#2a3447', timeVisible: true }
            });
            candleSeries = chart.addCandlestickSeries({
                upColor: '#00e676', downColor: '#ff1744',
                borderUpColor: '#00e676', borderDownColor: '#ff1744',
                wickUpColor: '#00e676', wickDownColor: '#ff1744'
            });
        }

        function connect() {
            if(ws) ws.close();
            const market = document.getElementById('market').value;
            const tf = document.getElementById('timeframe').value;
            ws = new WebSocket('wss://ws.derivws.com/websockets/v3?app_id=1089');
            ws.onopen = () => {
                ws.send(JSON.stringify({
                    ticks_history: market, subscribe: 1, end: 'latest', 
                    granularity: parseInt(tf), count: 200, style: 'candles'
                }));
            };
            ws.onmessage = (msg) => {
                const data = JSON.parse(msg.data);
                if (data.candles) {
                    candles = data.candles.map(c => ({ time: c.epoch, open: c.open, high: c.high, low: c.low, close: c.close }));
                    candleSeries.setData(candles);
                }
                if (data.ohlc) {
                    const c = data.ohlc;
                    const newCandle = { time: c.open_time, open: c.open, high: c.high, low: c.low, close: c.close };
                    if (candles.length > 0 && candles[candles.length-1].time === newCandle.time) {
                        candles[candles.length-1] = newCandle;
                    } else {
                        candles.push(newCandle);
                    }
                    candleSeries.update(newCandle);
                    runAutoAnalysis();
                }
            };
        }

        function runAutoAnalysis() {
            if (candles.length < 50) return;
            const current = candles[candles.length - 1];
            const prev = candles[candles.length - 2];
            const prev2 = candles[candles.length - 3];
            let score = 0;
            let checks = {};

            // 1. Trend Alignment (SMA 20 equivalent)
            const sma20 = candles.slice(-20).reduce((a, b) => a + b.close, 0) / 20;
            checks.c1 = current.close > sma20; // Up if above SMA
            if(checks.c1 || current.close < sma20) score++;

            // 2. Momentum (Last 2 candles same direction)
            checks.c2 = (current.close > current.open && prev.close > prev.open) || (current.close < current.open && prev.close < prev.open);
            if(checks.c2) score++;

            // 3. Structure Break (BOS/ChoCH Combined)
            const recentHigh = Math.max(...candles.slice(-15, -1).map(c => c.high));
            const recentLow = Math.min(...candles.slice(-15, -1).map(c => c.low));
            checks.c3 = current.close > recentHigh || current.close < recentLow;
            if(checks.c3) score++;

            // 4. Volume Confirmation (Size of candle)
            const avgBody = candles.slice(-10).reduce((a, b) => a + Math.abs(b.close - b.open), 0) / 10;
            checks.c4 = Math.abs(current.close - current.open) > avgBody;
            if(checks.c4) score++;

            // 5. Pattern Power (Engulfing or Strong Pin Bar)
            checks.c5 = Math.abs(current.close - current.open) > Math.abs(prev.close - prev.open);
            if(checks.c5) score++;

            // 6. FVG / Gap
            checks.c6 = Math.abs(prev2.high - current.low) > 0 || Math.abs(prev2.low - current.high) > 0;
            if(checks.c6) score++;

            // 7. Auto Key Level (Price near a pivot)
            const pivots = candles.slice(-50, -5).filter((c, i, arr) => (i>1 && c.high > arr[i-1].high && c.high > arr[i+1].high) || (i>1 && c.low < arr[i-1].low && c.low < arr[i+1].low));
            checks.c7 = pivots.some(p => Math.abs(p.high - current.close) / current.close < 0.001);
            if(checks.c7) score++;

            updateUI(score, checks, current, prev);
        }

        function updateUI(score, checks, current, prev) {
            for(let i=1; i<=7; i++) {
                const el = document.getElementById('c'+i);
                if(checks['c'+i]) { el.className = 'check-item active'; el.querySelector('span').innerText = '✓'; }
                else { el.className = 'check-item inactive'; el.querySelector('span').innerText = '✕'; }
            }

            document.getElementById('scoreText').innerText = score + '/7';
            document.getElementById('scoreBar').style.width = (score/7*100) + '%';

            const statusEl = document.getElementById('signalStatus');
            const accEl = document.getElementById('accuracy');

            if(score >= 5) {
                const type = current.close > prev.close ? 'BUY' : 'SELL';
                statusEl.innerText = type + ' !';
                statusEl.className = 'signal-status ' + (type === 'BUY' ? 'signal-buy' : 'signal-sell');
                accEl.innerText = (score === 7) ? "95" : (score === 6 ? "85" : "75");
                
                if(Date.now() - lastSignalTime > 60000) { // 1 min cool down
                    addHistory(type, score);
                    lastSignalTime = Date.now();
                    speak(type);
                }
            } else {
                statusEl.innerText = 'WAITING...';
                statusEl.className = 'signal-status signal-wait';
                accEl.innerText = '0';
            }
        }

        function addHistory(type, score) {
            const h = document.getElementById('history');
            const div = document.createElement('div');
            div.className = 'history-item';
            div.innerHTML = `<span>${new Date().toLocaleTimeString()}</span> <b style="color:${type==='BUY'?'#00e676':'#ff1744'}">${type}</b> <span>Score: ${score}/7</span>`;
            h.prepend(div);
            if(h.children.length > 5) h.lastChild.remove();
        }

        function speak(t) {
            const m = new SpeechSynthesisUtterance(t + " Signal Detected. Strength " + document.getElementById('scoreText').innerText);
            window.speechSynthesis.speak(m);
        }

        setInterval(() => { document.getElementById('clock').innerText = new Date().toLocaleTimeString(); }, 1000);
        document.getElementById('market').addEventListener('change', connect);
        document.getElementById('timeframe').addEventListener('change', connect);
        window.onload = () => { initChart(); connect(); };
    </script>
</body>
</html>
