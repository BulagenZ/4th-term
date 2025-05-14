<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Поиск и выделение слова</title>
    <script src="https://cdn.jsdelivr.net/npm/pyodide@0.21.1/pyodide.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f6f9;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
            width: 100%;
            max-width: 500px;
        }
        h1 {
            text-align: center;
            font-size: 20px;
            color: #333;
        }
        textarea, input[type="text"] {
            width: 100%;
            padding: 10px;
            font-size: 16px;
            margin-top: 10px;
            margin-bottom: 20px;
            border-radius: 5px;
            border: 1px solid #ddd;
            resize: vertical;
        }
        button {
            width: 100%;
            padding: 12px;
            font-size: 16px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        button:hover {
            background-color: #45a049;
        }
        #highlighted-text {
            font-size: 16px;
            color: #333;
        }
        mark {
            background-color: yellow;
            color: black;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Поиск и выделение слова</h1>
        <textarea id="text" rows="6" placeholder="Введите текст здесь..."></textarea>
        <input type="text" id="word" placeholder="Введите слово для поиска..." />
        <button id="search-button">Выделить слово</button>
        <div id="highlighted-text"></div>
    </div>

    <script>
        loadPyodide().then(pyodide => {
            document.getElementById('search-button').addEventListener('click', function() {
                const text = document.getElementById('text').value;
                const word = document.getElementById('word').value;

                const pythonCode = `
text = '''${text}'''
word = '''${word}'''
highlighted_text = text.replace(word, '<mark>' + word + '</mark>')
highlighted_text
`;

                pyodide.runPythonAsync(pythonCode).then(result => {
                    document.getElementById('highlighted-text').innerHTML = result;
                });
            });
        });
    </script>
</body>
</html>
