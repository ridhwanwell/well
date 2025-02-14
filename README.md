<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>MQTT Web Client</title>
  <script src="https://unpkg.com/mqtt/dist/mqtt.min.js"></script>
</head>
<body>
  <h2>MQTT Web Client - Potentiometer Data</h2>
  <div>
    <h3>Data Potensiometer:</h3>
    <p id="potentiometerValue">Waiting for data...</p>
  </div>

  <script>
    // MQTT connection settings
    const mqttBroker = 'wss://broker.hivemq.com:8000/mqtt';  // Broker HiveMQ WebSocket
    const clientId = 'web-client-1';  // Unique client ID
    const topic = 'sensor/potentiometer';  // Topic yang digunakan oleh ESP32

    // Membuat koneksi MQTT ke broker
    const client = mqtt.connect(mqttBroker, {
      clientId: clientId,
      clean: true,
      connectTimeout: 4000,
      reconnectPeriod: 1000,
    });

    // Menampilkan pesan ketika berhasil terkoneksi
    client.on('connect', function () {
      console.log('Connected to MQTT broker!');
      // Subscribe ke topic 'sensor/potentiometer' untuk menerima data
      client.subscribe(topic, function (err) {
        if (err) {
          console.log('Failed to subscribe:', err);
        } else {
          console.log('Subscribed to topic:', topic);
        }
      });
    });

    // Ketika menerima pesan baru dari topic
    client.on('message', function (topic, message) {
      const potValue = message.toString();  // Pesan yang diterima dalam format string
      document.getElementById('potentiometerValue').innerText = 'Potentiometer Value: ' + potValue;
    });
  </script>
</body>
</html>
