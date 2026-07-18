<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DANANJAYA SIGNALS - Trading Dashboard</title>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lightweight-charts/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        :root {
            --bg-color: #090d14;
            --card-bg: #121721;
            --text-main: #ffffff;
            --text-dim: #848e9c;
            --green: #00ff88;
            --red: #ff4d4d;
            --accent: #3d85ff;
            --border: #1e2633;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            font-family: 'Inter', sans-serif;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
        }

        .dashboard {
            width: 1200px;
            display: grid;
            grid-template-columns: 1fr 350px;
            gap: 20px;
        }

        /* Header Styles */
        .header {
            grid-column: span 2;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: var(--card-bg);
            padding: 15px 25px;
            border-radius: 12px;
            border: 1px solid var(--border);
        }

        .logo-area h1 {
            margin: 0;
            font-size: 24px;
            letter-spacing: 1px;
        }

        .logo-area span {
            color: var(--green);
        }

        .market-status {
            font-size: 14px;
            color: var(--text-dim);
        }

        .status-dot {
            height: 10px;
            width: 10px;
            background-color: var(--green);
            border-radius: 50%;
            display: inline-block;
            margin-right: 5px;
            box-shadow: 0 0 10px var(--green);
        }

        /* Top Bar Info */
        .top-info {
            grid-column: span 2;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
        }

        .info-card {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 10px;
            border: 1px solid var(--border);
        }

        .label { color: var(--text-dim); font-size: 12px; margin-bottom: 5px; }
        .value { font-size: 18px; font-weight: bold; }
        .up { color: var(--green); }

        /* Left Column */
        .main-chart-area {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        #chart-container {
            background: var(--card-bg);
            height: 450px;
            border-radius: 12px;
            border: 1px solid var(--border);
            overflow: hidden;
            position: relative;
        }

        .history-table {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 20px;
            border: 1px solid var(--border);
        }

        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th { text-align: left; color: var(--text-dim); font-size: 12px; padding-bottom: 10px; }
        td { padding: 10px 0; border-top: 1px solid var(--border); font-size: 14px; }

        /* Right Column */
        .sidebar {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .signal-card {
            background: linear-gradient(145deg, #16202e, #121721);
            padding: 25px;
            border-radius: 15px;
            text-align: center;
            border: 1px solid var(--green);
        }

        .signal-type { font-size: 48px; font-weight: 800; color: var(--green); margin: 10px 0; }
        
        .confidence-box {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 10px;
            border: 1px solid var(--border);
        }

        .progress-bar {
            height: 12px;
            background: #1e2633;
            border-radius: 6px;
            margin-top: 10px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            width: 87%;
            background: var(--green);
            box-shadow: 0 0 15px var(--green);
        }

        .checklist {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 10px;
            font-size: 13px;
        }

        .check-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 8px;
        }

        .check-icon { color: var(--green); }

    </style>
</head>
<body>

<div class="dashboard">
    <!-- Header -->
    <header class="header">
        <div class="logo-area">
            <h1>DANANJAYA <span>SIGNALS</span></h1>
        </div>
        <div class="market-status">
            Time: <span id="clock">10:24:36 AM</span> | 
            <span class="status-dot"></span> MARKET OPEN
        </div>
    </header>

    <!-- Top Bar -->
