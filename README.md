<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Шыфр Віжэнэра (беларуская мова)</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
        }
        input, button {
            padding: 8px;
            margin: 5px 0;
            width: 100%;
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <h1>Шыфр Віжэнэра</h1>
    
    <label for="text">Тэкст:</label>
    <input type="text" id="text" placeholder="Увядзіце зашыфраваны тэкст...">
    
    <label for="key">Ключ:</label>
    <input type="text" id="key" placeholder="Увядзіце ключ...">
    
    <button onclick="decrypt()">Расшыфраваць</button>
    
    <label for="result">Вынік:</label>
    <input type="text" id="result" readonly>
    
    <script>
        const belarusianAlphabetLower = 'абвгдеёжзійклмнопрстуўфхцчшыьэюя';
        const belarusianAlphabetUpper = belarusianAlphabetLower.toUpperCase();

        function prepareKey(key, length) {
            const preparedKey = [];
            for (let i = 0; i < length; i++) {
                const char = key[i % key.length].toLowerCase();
                const index = belarusianAlphabetLower.indexOf(char);
                preparedKey.push(index !== -1 ? index : 0);
            }
            return preparedKey;
        }

        function processText(text, key, mode) {
            let result = '';
            const preparedKey = prepareKey(key, text.length);
            
            for (let i = 0; i < text.length; i++) {
                const char = text[i];
                let alphabet, isLower;
                
                if (belarusianAlphabetLower.includes(char.toLowerCase())) {
                    if (belarusianAlphabetLower.includes(char)) {
                        alphabet = belarusianAlphabetLower;
                        isLower = true;
                    } else {
                        alphabet = belarusianAlphabetUpper;
                        isLower = false;
                    }
                    
                    const originalPos = alphabet.indexOf(char);
                    const shift = preparedKey[i];
                    let newPos;
                    
                    if (mode === 'encrypt') {
                        newPos = (originalPos + shift) % alphabet.length;
                    } else {
                        newPos = (originalPos - shift + alphabet.length) % alphabet.length;
                    }
                    
                    result += alphabet[newPos];
                } else {
                    result += char;
                }
            }
            return result;
        }

        function decrypt() {
            const text = document.getElementById('text').value;
            const key = document.getElementById('key').value;
            if (!text || !key) {
                alert("Увядзіце тэкст і ключ!");
                return;
            }
            document.getElementById('result').value = processText(text, key, 'decrypt');
        }
    </script>
</body>
</html>
