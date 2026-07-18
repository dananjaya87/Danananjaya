<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Live Dashboard - DANANJAYA</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            background-color: #0d1117;
            color: white;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        .main-container {
            width: 90%;
            max-width: 900px;
            background: #161b22;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            border: 1px solid #30363d;
            text-align: center;
        }

        /* 1089 වෙනුවට DANANJAYA නම */
        .header-name {
            font-size: 36px;
            font-weight: bold;
            color: #00ffcc; /* වෙනත් කැපී පෙනෙන වර්ණයක් */
            text-transform: uppercase;
            letter-spacing: 5px;
            margin-bottom: 10px;
            text-shadow: 0 0 15px rgba(0, 255, 204, 0.6);
        }

        /* Live Chart Area */
        .chart-box {
            width: 100%;
            height: 350px;
            background: #0d1117;
            border-radius: 10px;
            margin-bottom: 30px;
            padding: 10px;
        }

        /* පල්ලෙහා තියෙන Boxes සැකසුම */
        .footer-layout {
            display: flex;
            justify-content: center;
            align-items: flex-end; /* පල්ලෙහාටම සමාන වෙන්න */
            gap: 20px;
            margin-top: 20px;
        }

        .large-box {
            width: 150px;
            height: 100px;
            background: linear-gradient(145deg, #1f2937, #111827);
            border: 2px solid #3b82f6;
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
        }

        .smallest-box {
            width: 80px;
            height: 50px;
            background: #30363d;
            border: 2px solid #8b5cf6;
            border-radius: 8px;
            font-size: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #aaa;
        }

        /* දිනය සහ වේලාව */
        .live-info {
            margin-top: 30px;
            padding: 10px;
            border-radius: 50px;
            background: rgba(255, 255, 255, 0.05);
            color: #ff9f43; /* ලස්සන වෙනත් වර්ණයක් */
            font-weight: bold;
            font-size: 18px;
            border: 1px dashed #ff9f43;
        }
    </style>
</head>
<body>

    <div class="main-container">
        <!-- නම වෙනස් කළ කොටස -->
        <div class="header-name">DANANJAYA</div>

        <!-- Live Chart එක -->
        <div class="chart-box">
            <canvas id="liveChart"></canvas>
        </div>

        <!-- Box සැකසුම -->
        <div class="footer-layout">
            <div class="large-box">DATA A</div>
            <div class="smallest-box">MINI</div>
            <div class="large-box">DATA B</div>
        </div>

        <!-- දිනය සහ වේලාව -->
        <div class="live-info" id="dateTime">Loading Time...</div>
    </div>

    <script>
        // Live Chart එක සෑදීම
        const ctx = document.getElementById('liveChart').getContext('2d');
        let chartData = Array.from({length: 12}, () => Math.floor(Math.random() * 100));
        
        const myChart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: ['1s', '2s', '3s', '4s', '5s', '6s', '7s', '8s', '9s', '10s', '11s', '12s'],
                datasets: [{
                    label: 'Live Market Flow',
                    data: chartData,
                    borderColor: '#00ffcc',
                    borderWidth: 3,
                    tension: 0.4,
                    fill: true,
                    backgroundColor: 'rgba(0, 255, 204, 0.1)'
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: { display: false },
                    x: { grid: { color: '#30363d' } }
                },
                plugins: { legend: { display: false } }
            }
        });

        // Chart එක සජීවීව update කිරීම
        setInterval(() => {
            chartData.shift();
            chartData.push(Math.floor(Math.random() * 100));
            myChart.update();
        }, 2000);

        // දිනය සහ වේලාව සජීවීව පෙන්වීම
        function updateTime() {
            const now = new Date();
            const options = { 
                year: 'numeric', month: 'long', day: 'numeric',
                hour: '2-digit', minute: '2-digit', second: '2-digit'
            };
            document.getElementById('dateTime').innerHTML = "📅 " + now.toLocaleDateString('en-US', options);
        }
        setInterval(updateTime, 1000);
        updateTime();
    </script>

</body>
</html>
