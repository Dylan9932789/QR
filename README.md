head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Генератор QR-кода для видео</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcode-generator/1.4.4/qrcode.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }
        input[type="text"], button {
            padding: 10px;
            margin: 5px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        input[type="text"] {
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
    font-weight: bold; /* Жирный шрифт */
    margin-bottom: 5px; /* Отступ снизу */
}



        code {
            font-family: 'Courier New', Courier, monospace;
            font-size: 14px;
            background-color: #f4f4f4;
            padding: 2px 5px;
            border-radius: 4px;
        }
        nav ul li a {
            color: #333;
            text-decoration: none;
            padding: 5px 10px;
            border-radius: 5px;
            transition: background-color 0.3s ease;
        }

        nav ul li a:hover {
            background-color: #ccc;
        }

        section {
            margin: 20px 0;
        }

        h2 {
            color: #007bff;
        }

        p {
            line-height: 1.6;
        }

        footer p {
            margin: 0;
        }

        footer a {
            color: #fff;
            text-decoration: none;
        }

        footer a:hover {
            text-decoration: underline;
        }
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
            text-align: center; /* Центрирование QR-кода */
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
        }
        #dropArea.hover {
            border-color: #007bff;
        }
        #dropArea p {
            margin: 0;
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
                function scanQRCode() {
            var fileInput = document.getElementById('fileInput');
            var file = fileInput.files[0];
            if (file) {
                var reader = new FileReader();
                reader.onload = function(e) {
                    var image = new Image();
                    image.onload = function() {
                        var canvas = document.createElement('canvas');
                        var context = canvas.getContext('2d');
                        canvas.width = image.width;
                        canvas.height = image.height;
                        context.drawImage(image, 0, 0, image.width, image.height);
                        try {
                            var result = qrcode.decode();
                            alert('Содержимое QR-кода: ' + result);
                        } catch (error) {
                            alert('QR-код не найден или не может быть прочитан');
                        }
                    };
                    image.src = e.target.result;
                };
                reader.readAsDataURL(file);
            } else {
                alert('Выберите изображение для сканирования');
            }
        }
        function downloadQRCode() {
            var qrContainer = document.getElementById('qrcode');
            var qrImage = qrContainer.querySelector('img');
            
            // Создаем ссылку для скачивания
            var link = document.createElement('a');
            link.href = qrImage.src;
            link.download = 'qrcode.png'; // Имя файла для скачивания
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        }
        document.getElementById('fileInput').addEventListener('change', scanQRCode);
        function handleDragOver(event) {
        event.preventDefault();
        event.dataTransfer.dropEffect = 'copy';
        // Добавляем класс hover при наведении элемента на область
        document.getElementById('dropArea').classList.add('hover');
    }

    function handleDrop(event) {
        event.preventDefault();
        // Убираем класс hover после завершения операции перетаскивания
        document.getElementById('dropArea').classList.remove('hover');

        var file = event.dataTransfer.files[0];
        var reader = new FileReader();
        reader.onload = function(e) {
            var image = new Image();
            image.src = e.target.result;
            // Отображаем изображение в элементе #qrcode
            document.getElementById('qrcode').innerHTML = '';
            document.getElementById('qrcode').appendChild(image);
        };
        reader.readAsDataURL(file);
    }
    </script>

    <footer>
                <p>&copy; 2024 Разработчик Dylan933. Все права защищены.  г.Вяземский| </p>
    </footer>
