<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Letter or Word Search</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
    }
    textarea {
      width: 100%;
      height: 150px;
      margin: 10px 0;
      padding: 8px;
      border: 1px solid #ccc;
      border-radius: 4px;
      resize: vertical;
      box-sizing: border-box;
    }
    #output {
      margin-top: 20px;
      white-space: pre-wrap;
      background-color: #f0f0f0;
      padding: 10px;
      border-radius: 5px;
      min-height: 50px;
    }
    strong {
      background-color: yellow;
      font-weight: normal;
    }
    .error {
      color: red;
    }
    .controls {
      margin: 10px 0;
    }
    button {
      padding: 8px 16px;
      background-color: #4CAF50;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>
  <h2>Search Letter or Word in Text</h2>
  <label for="textInput">Enter text to search:</label>
  <textarea id="textInput" placeholder="Write or paste your text here..."></textarea>
  <div class="controls">
    <label for="wordInput">Letter or word to search:</label>
    <input type="text" id="wordInput">
    <label><input type="checkbox" id="caseSensitive"> Case sensitive</label>
    <label><input type="checkbox" id="wholeWord"> Whole word only</label>
  </div>
  <button onclick="searchText()">Search</button>

  <div id="output"></div>

  <script>
    function escapeRegExp(string) {
      return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
    }

    function searchText() {
      const text = document.getElementById('textInput').value;
      const word = document.getElementById('wordInput').value.trim();
      const caseSensitive = document.getElementById('caseSensitive').checked;
      const wholeWord = document.getElementById('wholeWord').checked;
      const output = document.getElementById('output');

      // Reset output
      output.innerHTML = '';
      output.classList.remove('error');

      // Validate inputs
      if (!text) {
        output.classList.add('error');
        output.textContent = 'Error: Please enter text to search.';
        return;
      }
      if (!word) {
        output.classList.add('error');
        output.textContent = 'Error: Please enter a letter or word to search for.';
        return;
      }

      try {
        const escapedWord = escapeRegExp(word);
        let regexPattern = wholeWord ? `\\b${escapedWord}\\b` : escapedWord;
        let regexFlags = caseSensitive ? 'g' : 'gi';
        const regex = new RegExp(regexPattern, regexFlags);
        const matches = text.match(regex);
        if (matches) {
          const highlighted = text.replace(regex, '<strong>$&</strong>');
          output.innerHTML = highlighted;
        } else {
          output.textContent = 'No matches found.';
        }
      } catch (err) {
        output.classList.add('error');
        output.textContent = 'Error: Invalid search term.';
      }
    }
  </script>
</body>
</html>
