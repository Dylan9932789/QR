
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Генератор QR-кода для видео</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcode-generator/1.4.4/qrcode.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/jsqr/dist/jsQR.js"></script>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #f0f0f0;
        margin: 0;
        padding: 0;
    }
    input[type="text"], input[type="file"], button {
        padding: 10px;
        margin: 5px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }
    input[type="text"], input[type="file"] {
        width: 300px;
    }
    button {
        background-color: #007bff;
        color: #fff;
    }
    button:hover {
        background-color: #0056b3;
    }
    label {
        margin-left: 5px;
    }
    #qrcode {
        margin-top: 20px;
        text-align: center;
    }
    .modal {
        display: none;
        position: fixed;
        z-index: 1;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        overflow: auto;
        background-color: rgba(0,0,0,0.8);
    }
    .modal-content {
        margin: 10% auto;
        width: 80%;
        max-width: 600px;
        background-color: #fff;
        padding: 20px;
        position: relative;
        border-radius: 10px;
    }
    .close {
        position: absolute;
        top: 10px;
        right: 10px;
        font-size: 20px;
        cursor: pointer;
    }
    footer {
        position: fixed;
        bottom: 0;
        left: 0;
        width: 100%;
        background-color: #333;
        color: #fff;
        padding: 10px;
        text-align: center;
    }
    .qrSizeLabel {
        display: block;
        margin-top: 10px;
        font-size: 16px;
        color: #333;
        font-weight: bold;
        margin-bottom: 5px;
    }
    .downloadButton {
        background-color: #28a745;
    }
    .chooseFileButton {
        background-color: #17a2b8;
    }
    /* Стили для поля перетаскивания */
    #dropArea {
        border: 2px dashed #ccc;
        border-radius: 5px;
        padding: 20px;
        text-align: center;
        margin-top: 20px;
        cursor: pointer; /* Добавляем курсор для указания на возможность перетаскивания */
    }
    #dropArea.hover {
        border-color: #007bff;
    }
    #dropArea p {
        margin: 0;
    }
    /* Стили для кнопки копирования считанной ссылки */
    #copyScannedLinkButton {
        display: block;
        margin: 10px auto;
        padding: 10px 20px;
        border: none;
        border-radius: 5px;
        background-color: #007bff;
        color: #fff;
        cursor: pointer;
        transition: background-color 0.3s ease;
    }
    #copyScannedLinkButton:hover {
        background-color: #0056b3;
    }
</style>
</head>
<body>
<input id="videoLink" type="text" placeholder="Введите ссылку на сайт">
<button onclick="generateVideoQR()">Создать QR-код</button>
<button onclick="pasteFromClipboard()">Открыть</button>
<button onclick="clearQRCode()">Очистить</button>
<button onclick="copyToClipboard()">Копировать в буфер</button>
<button onclick="pasteVideoLinkFromClipboard()">Вставить ссылку</button>
<button class="downloadButton" onclick="downloadQRCode()">Скачать</button>
<label for="qrSize">Размер QR-кода:</label>
<button onclick="copyScannedDataToClipboard()">Копировать считанную ссылку</button>
<input id="qrSize" type="number" min="100" max="500" value="200">

<input type="file" id="fileInput">

<div id="dropArea" ondrop="handleDrop(event)" ondragover="handleDragOver(event)">
    <p>Перетащите сюда изображение для сканирования QR-кода</p>
</div>
<div id="qrcode"></div>

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
<a href="Дилан браузер9" download>Download link</a>
<footer>
    <p>&copy; 2024 Разработчик Dylan933. Все права защищены. г.Вяземский| </p>
</footer>
