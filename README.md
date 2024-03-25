

<head>
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
        code {
    font-family: 'Courier New', Courier, monospace;
    font-size: 14px;
    background-color: #f4f4f4;
    padding: 2px 5px;
    border-radius: 4px;
}
    </style>
</head>
<body>
    <input id="videoLink" type="text" placeholder="Введите ссылку на видео">
    <button onclick="generateVideoQR()">Создать QR-код</button>
    <button onclick="pasteFromClipboard()">Вставить</button>
    <button onclick="clearQRCode()">Очистить</button>
    <button onclick="copyToClipboard()">Копировать в буфер</button>
    <br>
    <label for="qrSize">Размер QR-кода:</label>
    <input id="qrSize" type="number" min="100" max="500" value="200">
    
 
    
    <div id="qrcode"></div>

   
    <div id="videoModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeVideoModal()">&times;</span>
            <iframe id="videoFrame" width="560" height="315" frameborder="0" allowfullscreen></iframe>
        </div>
    </div>

   

    <footer>
        <p>&copy; 2024 Ваша Компания. Все права защищены. | <span id="companyLink">www.example.com</span></p>
       
