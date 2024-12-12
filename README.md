
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Генератор QR-кода </title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcode-generator/1.4.4/qrcode.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/jsqr/dist/jsQR.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            background: linear-gradient(120deg, #f0f0f0, #ffffff);
            font-family: 'Roboto', sans-serif;
            margin: 0;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        h1 {
            margin: 10px;
            color: #333;
        }
        .qr-container {
            text-align: center;
            margin: 20px 0;
        }
        .qr-code {
            max-width: 300px;
            width: 100%;
            margin: 20px auto;
        }
        input[type="text"], input[type="number"] {
            width: calc(100% - 20px);
            max-width: 300px;
            padding: 10px;
            font-size: 16px;
            margin: 10px 0;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        button {
            background-color: #0078FF;
            color: #fff;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            margin: 5px;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #0056CC;
        }
        #dropArea {
            border: 2px dashed #0078FF;
            border-radius: 10px;
            padding: 20px;
            text-align: center;
            margin: 20px 0;
            width: calc(100% - 20px);
            max-width: 300px;
        }
        #dropArea.hover {
            background-color: #e6f7ff;
        }
        footer {
            text-align: center;
            margin-top: 20px;
            font-size: 14px;
            color: #666;
        }
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            position: relative;
        }
        .modal-content iframe {
            max-width: 100%;
            border-radius: 10px;
        }
        .modal .close {
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 20px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Генератор QR-кода </h1>
    <input id="videoLink" type="text" placeholder="Введите ссылку ">
    <button onclick="generateVideoQR()">Создать QR-код</button>
    <button onclick="clearQRCode()">Очистить</button>
    <button onclick="pasteVideoLinkFromClipboard()">Вставить ссылку</button>
    <button class="downloadButton" onclick="downloadQRCode()">Скачать</button>
    <button onclick="copyScannedDataToClipboard()">Копировать считанную ссылку</button>
    <label for="qrSize">Размер QR-кода:</label>
    <input id="qrSize" type="number" min="100" max="500" value="200">
    <div id="dropArea" ondrop="handleDrop(event)" ondragover="handleDragOver(event)">
        <p>Перетащите сюда изображение для сканирования QR-кода</p>
    </div>
    <div id="qrcode" class="qr-container"></div>
    <div id="videoModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeVideoModal()">&times;</span>
            <iframe id="videoFrame" width="560" height="315" frameborder="0" allowfullscreen></iframe>
        </div>
    </div>
        <script>
    var scannedData = ''; // Глобальная переменная для хранения считанной информации
    function generateVideoQR() {
        var videoLink = document.getElementById('videoLink').value;
        var qrSize = document.getElementById('qrSize').value;
        var qr = qrcode(0, 'M');
        qr.addData(videoLink);
        qr.make();
        var qrCanvas = document.createElement('canvas');
        qrCanvas.width = qrSize;
        qrCanvas.height = qrSize;
        var qrContext = qrCanvas.getContext('2d');
        var moduleCount = qr.getModuleCount();
        var moduleSize = qrSize / moduleCount;
        for (var row = 0; row < moduleCount; row++) {
            for (var col = 0; col < moduleCount; col++) {
                if (qr.isDark(row, col)) {
                    qrContext.fillStyle = "#000"; // Установка цвета для заменяющего пикселя
                } else {
                    qrContext.fillStyle = "#fff"; // Установка цвета для фона
                }
                qrContext.fillRect(col * moduleSize, row * moduleSize, moduleSize, moduleSize);
            }
        }
        var qrImage = document.createElement('img');
        qrImage.src = qrCanvas.toDataURL('image/png');
        var qrContainer = document.getElementById('qrcode');
        qrContainer.innerHTML = '';
        qrContainer.appendChild(qrImage);
    }
    function clearQRCode() {
        document.getElementById('videoLink').value = '';
        document.getElementById('qrcode').innerHTML = '';
    }
    function copyToClipboard() {
        var qrContainer = document.getElementById('qrcode');
        var qrImage = qrContainer.querySelector('img');
        var tempInput = document.createElement('input');
        tempInput.setAttribute('value', qrImage.src);
        document.body.appendChild(tempInput);
        tempInput.select();
        document.execCommand('copy');
        document.body.removeChild(tempInput);
        alert('QR-код скопирован в буфер обмена!');
    }
    function pasteFromClipboard() {
        navigator.clipboard.readText().then(function(text) {
            document.getElementById('videoLink').value = text;
            openVideoModal(text);
        });
    }
    function pasteVideoLinkFromClipboard() {
        navigator.clipboard.readText().then(function(text) {
            document.getElementById('videoLink').value = text;
            generateVideoQR();
        });
    }
    function openVideoModal(videoLink) {
        var modal = document.getElementById('videoModal');
        var videoFrame = document.getElementById('videoFrame');
        videoFrame.src = videoLink;
        modal.style.display = 'block';
    }
    function closeVideoModal() {
        var modal = document.getElementById('videoModal');
        var videoFrame = document.getElementById('videoFrame');
        videoFrame.src = '';
        modal.style.display = 'none';
    }
    window.onclick = function(event) {
        var modal = document.getElementById('videoModal');
        if (event.target == modal) {
            closeVideoModal();
        }
    };
    function scanQRCodeFromFile(file) {
        var reader = new FileReader();
        reader.onload = function(e) {
            var imageData = e.target.result;
            var image = new Image();
            image.onload = function() {
                var canvas = document.createElement('canvas');
                var context = canvas.getContext('2d');
                canvas.width = image.width;
                canvas.height = image.height;
                context.drawImage(image, 0, 0, image.width, image.height);
                var imageData = context.getImageData(0, 0, canvas.width, canvas.height);
                var code = jsQR(imageData.data, imageData.width, imageData.height);
                if (code) {
                    scannedData = code.data; // Сохраняем считанную информацию
                    alert('Содержимое QR-кода: ' + scannedData);
                } else {
                    alert('QR-код не найден или не может быть прочитан');
                }
            };
            image.src = imageData;
        };
        reader.readAsDataURL(file);
    }
    function copyScannedDataToClipboard() {
        var tempInput = document.createElement('input');
        tempInput.setAttribute('value', scannedData);
        document.body.appendChild(tempInput);
        tempInput.select();
        document.execCommand('copy');
        document.body.removeChild(tempInput);
        alert('Скопировано в буфер обмена: ' + scannedData);
    }
    function handleDrop(event) {
        event.preventDefault();
        document.getElementById('dropArea').classList.remove('hover');
        var file = event.dataTransfer.files[0];
        scanQRCodeFromFile(file);
    }
    function handleDragOver(event) {
        event.preventDefault();
        event.dataTransfer.dropEffect = 'copy';
        document.getElementById('dropArea').classList.add('hover');
    }
    function downloadQRCode() {
        var qrContainer = document.getElementById('qrcode');
        var qrImage = qrContainer.querySelector('img');
        var link = document.createElement('a');
        link.href = qrImage.src;
        link.download = 'qrcode.png';
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
    }
    document.getElementById('fileInput').addEventListener('change', function() {
        var file = this.files[0];
        scanQRCodeFromFile(file);
    });

</script>
        
  


