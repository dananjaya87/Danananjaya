<!DOCTYPE html>
<html lang="si">
<head>
    <meta charset="UTF-8">
    <title>1089 ULTIMATE AUTO-SIGNAL ENGINE</title>
    <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Rajdhani:wght@500;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --bg: #05070a; --panel: #0d1117; --neon-blue: #00d2ff;
            --neon-green: #39ff14; --neon-red: #ff3131; --text: #e0e6ed; --border: #1a222d;
        }
        body { margin: 0; font-family: 'Rajdhani', sans-serif; background: var(--bg); color: var(--text); display: flex; flex-direction: column; height: 100vh; overflow: hidden; }
        
        .header { background: var(--panel); padding: 10px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid var(--border); }
        .logo { font-family: 'Orbitron', sans-serif; font-size: 1.2rem; color: var(--neon-blue); text-shadow: 0 0 10px var(--neon-blue); }

        .container { display: grid; grid-template-columns: 1fr 380px; gap: 10px; padding: 10px; flex: 1; overflow: hidden; }
        
        /* Main Signal Area */
        .signal-view { background: var(--panel); border-radius: 15px; border: 1px solid var(--border); display: flex; flex-direction: column; align-items: center; justify-content: center; position: relative; }
        .acc-display { font-family: 'Orbitron', sans-serif; font-size: 5.5rem; color: var(--neon-green); margin-bottom: 0; }
        .sig-status { font-size: 2.2rem; font-weight: 800; text-transform: uppercase; letter-spacing: 2px; }
        
        /* Sidebar Rules */
        .rules-sidebar { display: flex; flex-direction: column; gap: 10px; overflow-y: auto; }
        .rule-group { background: var(--panel); padding: 15px; border-radius: 12px; border: 1px solid var(--border); }
        .group-title { font-size: 0.75rem; color: #848e9c; text-transform: uppercase; letter-spacing: 1.5px; margin-bottom: 10px; border-bottom: 1px solid #1a222d; padding-bottom: 5px; }
        
        .indicator { background: #161b22; padding: 10px; margin-bottom: 6px; border-radius: 6px; display: flex; justify-content: space-between; font-size: 0.9rem; border-left: 3px solid #333; transition: 0.3s; }
        .indicator.active-setup { border-left-color: var(--neon-green); color: white; background: rgba(57, 255, 20, 0.05); }
        .indicator.active-confirm { border-left-color: var(--neon-blue); color: white; background: rgba(0, 210, 255, 0.05); }

        .log-window { height: 120px; background: #000; color: #39ff14; font-family: monospace; font-size: 0.75rem; padding: 8px; border-radius: 8px; overflow-y: auto; margin-top: 5px; }

        /* Animations */
        .pulse-buy { animation: pulse-g 1.5s infinite; }
        .pulse-sell { animation: pulse-r 1.5s infinite; }
        @keyframes pulse-g { 0%, 100% { box-shadow: inset 0 0 10px rgba(57, 255, 20, 0); } 50% { box-shadow: inset 0 0 40px rgba(57, 255, 20, 0.2); } }
        @keyframes pulse-r { 0%, 100% { box-shadow: inset 0 0 10px rgba(255, 49, 49, 0); } 50% { box-shadow: inset 0 0 40px rgba(255, 49, 49, 0.2); } }
    </style>
</head>
<body>

    <div class="header">
        <div class="logo">1089 AI ENGINE AUTO-SCAN</div>
        <div id="connection" style="color:var(--neon-blue)">INITIALIZING...</div>
    </div>

    <div class="container">
        <!-- Center Signal View -->
        <div class="signal-view" id="mainBox">
            <div style="color: #848e9c; font-size: 0.9rem; letter-spacing: 4px;">CONFIDENCE LEVEL</div>
            <div class="acc-dial acc-display" id="accuracy">00%</div>
            <div class="sig-status" id="signalLabel">WAITING...</div>
            <div id="price" style="margin-top: 20px; font-weight: bold; color: var(--neon-blue);">PRICE: --.----</div>
        </div>

        <!-- Right Sidebar Boxes -->
        <div class="rules-sidebar">
            <!-- Box 1: The 5 Techniques (Setups) -->
            <div class="rule-group">
                <div class="group-title">Setup Indicators (ප්‍රධාන සාධක 5)</div>
                <div class="indicator" id="s1">1. One Colour Streak <span>-</span></div>
                <div class="indicator" id="s2">2. Proper Engulfing <span>-</span></div>
                <div class="indicator" id="s3">3. Key Level (Round) <span>-</span></div>
                <div class="indicator" id="s4">4. Valid FVG Zone <span>-</span></div>
                <div class="indicator" id="s5">5. Hidden Open Point <span>-</span></div>
            </div>

            <!-- Box 2: Confirmations (අමතර සාධක) -->
            <div class="rule-group" style="border-color: var(--neon-blue);">
                <div class="group-title" style="color: var(--neon-blue);">Confirmations (අමතර සාධක)</div>
                <div class="indicator" id="c1">BOS (Break of Structure) <span>-</span></div>
                <div class="indicator" id="c2">ChoCh (Change Direction) <span>-</span></div>
                <div class="indicator" id="c3">Trend Line Alignment <span>-</span></div>
            </div>

            <div class="log-window" id="logs">Booting 1089 System...</div>
        </div>
    </div>

    <script>
        const app_id = 1089;
        const ws = new WebSocket(`wss://ws.binaryws.com/websockets/v3?app_id=${app_id}`);
        let candles = [];
        let high_peak = 0; let low_peak = 999999;

        ws.onopen = () => {
            document.getElementById('connection').innerText = "CONNECTED";
            ws.send(JSON.stringify({ ticks_history: "R_100", count: 100, end: "latest", style: "candles", subscribe: 1 }));
            addLog("Connected to Volatility 100...");
        };

        ws.onmessage = (msg) => {
            const data = JSON.parse(msg.data);
            if (data.candles) candles = data.candles;
            if (data.ohlc) {
                if (candles.length > 0 && candles[candles.length - 1].epoch === data.ohlc.open_time) {
                    candles[candles.length - 1] = data.ohlc;
                } else {
                    candles.push(data.ohlc);
                    if (candles.length > 100) candles.shift();
                }
                run1089Logic();
            }
        };

        function run1089Logic() {
            if (candles.length < 10) return;
            const cur = candles[candles.length-1];
            const prev = candles[candles.length-2];
            const p2 = candles[candles.length-3];
            const p3 = candles[candles.length-4];

            document.getElementById('price').innerText = `LIVE PRICE: ${cur.close}`;

            let setupCount = 0; let confirmCount = 0;

            // --- SETUPS LOGIC ---
            // S1: One Color (3 candles)
            const isUp = (c) => c.close > c.open;
            if((isUp(cur) && isUp(prev) && isUp(p2)) || (!isUp(cur) && !isUp(prev) && !isUp(p2))) { setupCount++; setOn('s1', 'setup'); } else setOff('s1');

            // S2: Engulfing
            if(isUp(cur) && !isUp(prev) && cur.close > prev.open) { setupCount++; setOn('s2', 'setup'); }
            else if(!isUp(cur) && isUp(prev) && cur.close < prev.open) { setupCount++; setOn('s2', 'setup'); } else setOff('s2');

            // S3: Round Number
            if(Math.abs(cur.close - Math.round(cur.close)) < 0.1) { setupCount++; setOn('s3', 'setup'); } else setOff('s3');

            // S4: FVG
            if(p2.high < cur.low || p2.low > cur.high) { setupCount++; setOn('s4', 'setup'); } else setOff('s4');

            // S5: Hidden Open Point
            if(Math.abs(cur.open - prev.close) > 0.02) { setupCount++; setOn('s5', 'setup'); } else setOff('s5');

            // --- CONFIRMATION LOGIC ---
            // C1: BOS
            if(cur.close > high_peak) { high_peak = cur.high; confirmCount++; setOn('c1', 'confirm'); }
            else if(cur.close < low_peak) { low_peak = cur.low; confirmCount++; setOn('c1', 'confirm'); } else setOff('c1');

            // C2: ChoCh (Simple reverse of trend)
            if((isUp(cur) && !isUp(prev) && !isUp(p2)) || (!isUp(cur) && isUp(prev) && isUp(p2))) { confirmCount++; setOn('c2', 'confirm'); } else setOff('c2');

            // C3: Trend Line (Last 10 slope)
            let slope = cur.close - candles[candles.length-10].close;
            if((slope > 0 && isUp(cur)) || (slope < 0 && !isUp(cur))) { confirmCount++; setOn('c3', 'confirm'); } else setOff('c3');

            // --- FINAL SIGNAL CALCULATION ---
            const mainBox = document.getElementById('mainBox');
            const accVal = document.getElementById('accuracy');
            const label = document.getElementById('signalLabel');

            let totalAcc = (setupCount * 12) + (confirmCount * 13);
            if(totalAcc > 99) totalAcc = 99;
            accVal.innerText = totalAcc + "%";

            // සිග්නල් එකක් දෙන්නේ බොක්ස් දෙකේම අවම වශයෙන් එකක්වත් DETECT වුණොත් පමණි.
            if(setupCount >= 1 && confirmCount >= 1) {
                const side = isUp(cur) ? "BUY" : "SELL";
                label.innerText = "STRONG " + side;
                label.style.color = isUp(cur) ? "var(--neon-green)" : "var(--neon-red)";
                mainBox.className = "signal-view " + (isUp(cur) ? "pulse-buy" : "pulse-sell");
                if(totalAcc > 70) addLog(`>>> ALERT: ${totalAcc}% ${side} SIGNAL FOUND!`);
            } else {
                label.innerText = "SCANNING...";
                label.style.color = "#fff";
                mainBox.className = "signal-view";
            }
        }

        function setOn(id, type) { 
            const e = document.getElementById(id); 
            e.className = 'indicator active-' + type; 
            e.querySelector('span').innerText = "DETECTED"; 
        }
        function setOff(id) { 
            const e = document.getElementById(id); 
            e.className = 'indicator'; 
            e.querySelector('span').innerText = "-"; 
        }
        function addLog(m) { 
            const l = document.getElementById('logs'); 
            l.innerHTML += `<br>> ${m}`; 
            l.scrollTop = l.scrollHeight; 
        }
    </script>
</body>
</html>
