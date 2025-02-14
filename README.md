<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Potensiometer Value</title>
    <script src="https://unpkg.com/mqtt/dist/mqtt.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin-top: 50px;
        }
        #potValue {
            font-size: 24px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>Nilai Potensiometer</h1>
    <div id="potValue">0</div>

    <script>
        // Ganti dengan informasi broker MQTT Anda
        const mqttServer = 'ws://broker.hivemq.com:8000/mqtt'; // HiveMQ WebSocket
        const potensiometerTopic = 'esp32/potensiometer';

        // Koneksi ke broker MQTT
        const client = mqtt.connect(mqttServer);

        client.on('connect', function () {
            console.log('Connected to MQTT broker');
            client.subscribe(potensiometerTopic, function (err) {
                if (!err) {
                    console.log('Subscribed to ' + potensiometerTopic);
                }
            });
        });

        client.on('message', function (topic, message) {
            // message adalah Buffer, kita perlu mengubahnya menjadi string
            const potValue = message.toString();
            document.getElementById('potValue').innerText = potValue;
        });
    </script>
</body>
</html>
