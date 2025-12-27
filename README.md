<!DOCTYPE html>
<html>
<head>
    <title>Water Quality Monitoring System</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: Arial;
            background: #f4f6f9;
            text-align: center;
        }
        .card {
            background: white;
            padding: 15px;
            margin: 10px;
            display: inline-block;
            width: 200px;
            border-radius: 10px;
            box-shadow: 0 0 10px #ccc;
        }
        .alert {
            font-size: 22px;
            color: red;
            font-weight: bold;
        }
    </style>
</head>
<body>

<h1>💧 Water Quality Monitoring System</h1>

<div class="card">pH: <span id="ph">--</span></div>
<div class="card">Turbidity: <span id="turbidity">--</span> NTU</div>
<div class="card">TDS: <span id="tds">--</span> ppm</div>
<div class="card">Temperature: <span id="temp">--</span> °C</div>

<p id="alert" class="alert"></p>

<canvas id="chart" width="600" height="300"></canvas>

<script>
let labels = [];
let phData = [];

const ctx = document.getElementById('chart').getContext('2d');
const chart = new Chart(ctx, {
    type: 'line',
    data: {
        labels: labels,
        datasets: [{
            label: 'pH Level',
            data: phData,
            borderColor: 'blue',
            fill: false
        }]
    }
});

function fetchData() {
    fetch('/data')
        .then(res => res.json())
        .then(data => {
            document.getElementById("ph").innerText = data.ph;
            document.getElementById("turbidity").innerText = data.turbidity;
            document.getElementById("tds").innerText = data.tds;
            document.getElementById("temp").innerText = data.temperature;
            document.getElementById("alert").innerText = data.alert;

            labels.push(data.time);
            phData.push(data.ph);

            if (labels.length > 10) {
                labels.shift();
                phData.shift();
            }

            chart.update();
        });
}

setInterval(fetchData, 2000);
</script>

</body>
</html>
