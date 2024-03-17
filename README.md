<<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QR Code Generator and Scanner</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
        }
        h1, h2 {
            color: #333;
        }
        input, button {
            margin: 10px;
            padding: 5px;
        }
        #qrcode {
            margin-top: 20px;
        }
        video, canvas {
            display: block;
            margin: 10px auto;
        }
    </style>
</head>
<body>
    <h1>QR Code Generator and Scanner</h1>

    <h2>Generate QR Code</h2>
    <input id="text" type="text" placeholder="Enter text or URL">
    <button onclick="generateQR()">Generate QR Code</button>
    <div id="qrcode"></div>

    <h2>Scan QR Code</h2>
    <video id="video" width="300" height="300" style="display: none;"></video>
    <canvas id="canvas" width="300" height="300" style="display: none;"></canvas>
    <button onclick="startScan()">Start Scanning</button>
    <div id="result"></div>

    <h2>Additional Actions</h2>
    <button onclick="clearQRCode()">Clear QR Code</button>
    <button onclick="downloadQRCode()">Download QR Code</button>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcode-generator/1.4.4/qrcode.min.js"></script>
    <script src="https://raw.githubusercontent.com/sitepoint-editors/jsqrcode/master/src/qr_packed.js"></script>
    <script>
        var videoFacingMode = 'environment';

        function generateQR() {
            var text = document.getElementById('text').value;
            if (text.trim() === "") {
                alert("Please enter text or URL to generate QR code.");
                return;
            }
            var typeNumber = 4;
            var errorCorrectionLevel = 'L';
            var qr = qrcode(typeNumber, errorCorrectionLevel);
            qr.addData(text);
            qr.make();
            document.getElementById('qrcode').innerHTML = qr.createImgTag();
        }

        function startScan() {
            var video = document.getElementById('video');
            var canvas = document.getElementById('canvas');
            var context = canvas.getContext('2d');
            var resultContainer = document.getElementById('result');

            navigator.mediaDevices.getUserMedia({ video: { facingMode: videoFacingMode } })
                .then(function(stream) {
                    video.srcObject = stream;
                    video.setAttribute('playsinline', true);
                    video.play();
                    requestAnimationFrame(tick);
                })
                .catch(function(error) {
                    console.error('Error accessing camera:', error);
                    alert('Could not access the camera. Please check camera permissions.');
                });

            function tick() {
                if (video.readyState === video.HAVE_ENOUGH_DATA) {
                    canvas.width = video.videoWidth;
                    canvas.height = video.videoHeight;
                    context.drawImage(video, 0, 0, canvas.width, canvas.height);
                    var imageData = context.getImageData(0, 0, canvas.width, canvas.height);
                    var code = jsQR(imageData.data, imageData.width, imageData.height, {
                        inversionAttempts: "dontInvert",
                    });
                    if (code) {
                        resultContainer.innerHTML = 'Scanned QR Code Data: ' + code.data;
                    }
                }
                requestAnimationFrame(tick);
            }
        }

        function clearQRCode() {
            document.getElementById('qrcode').innerHTML = '';
        }

        function downloadQRCode() {
            var qrcode = document.getElementById('qrcode').innerHTML;
            if (qrcode) {
                var blob = new Blob([qrcode], { type: 'image/svg+xml' });
                var url = URL.createObjectURL(blob);

                var a = document.createElement('a');
                a.href = url;
                a.download = 'qrcode.svg';
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
            } else {
                alert('No QR code generated to download. Please generate a QR code first.');
            }
        }
    </script>
</body>
</html>
<p>&copy; 2024 Разработчик Dylan933 Все права защищены. | <span id="companyLink"></span></p> \\
