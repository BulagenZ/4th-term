<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Letter or Word Highlighter</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
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
    }
    .error {
      color: red;
    }
    .loading {
      color: blue;
    }
    .controls {
      margin: 10px 0;
    }
    /* Custom file input styling */
    .file-input-wrapper {
      display: inline-block;
      position: relative;
      margin-bottom: 10px;
    }
    .file-input-wrapper input[type="file"] {
      opacity: 0;
      width: 0;
      height: 0;
      position: absolute;
    }
    .file-input-wrapper label {
      display: inline-block;
      padding: 8px 16px;
      background-color: #4CAF50;
      color: white;
      border-radius: 4px;
      cursor: pointer;
      text-align: center;
    }
    .file-input-wrapper label:hover {
      background-color: #45a049;
    }
  </style>
</head>
<body>
  <h2>Highlight Letter or Word in Uploaded File</h2>
  <div class="file-input-wrapper">
    <label for="fileInput">Select Text File</label>
    <input type="file" id="fileInput" accept=".txt">
  </div>
  <p><small>Supported file type: Plain text (.txt)</small></p>
  <div class="controls">
    <label for="wordInput">Letter or word to highlight:</label>
    <input type="text" id="wordInput">
    <label><input type="checkbox" id="caseSensitive"> Case sensitive</label>
    <label><input type="checkbox" id="wholeWord"> Whole word only</label>
  </div>
  <button onclick="processFile()">Highlight</button>

  <div id="output"></div>

  <script>
    function escapeRegExp(string) {
      return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
    }

    function processFile() {
      const fileInput = document.getElementById('fileInput');
      const word = document.getElementById('wordInput').value.trim();
      const caseSensitive = document.getElementById('caseSensitive').checked;
      const wholeWord = document.getElementById('wholeWord').checked;
      const output = document.getElementById('output');

      // Reset output
      output.innerHTML = '';
      output.classList.remove('error', 'loading');

      // Validate inputs
      if (!fileInput.files[0]) {
        output.classList.add('error');
        output.textContent = 'Error: Please upload a text file.';
        return;
      }
      if (!word) {
        output.classList.add('error');
        output.textContent = 'Error: Please enter a letter or word to highlight.';
        return;
      }

      // Check file size (limit to 1MB for safety)
      if (fileInput.files[0].size > 1024 * 1024) {
        output.classList.add('error');
        output.textContent = 'Error: File is too large (max 1MB).';
        return;
      }

      // Show loading message
      output.classList.add('loading');
      output.textContent = 'Processing file...';

      const reader = new FileReader();
      reader.onload = function(e) {
        try {
          let text = e.target.result;
          const escapedWord = escapeRegExp(word);
          let regexPattern = wholeWord ? `\\b${escapedWord}\\b` : escapedWord;
          let regexFlags = caseSensitive ? 'g' : 'gi';
          const regex = new RegExp(regexPattern, regexFlags);
          const highlighted = text.replace(regex, '<strong>$&</strong>');
          output.classList.remove('loading');
          output.innerHTML = highlighted;
        } catch (err) {
          output.classList.add('error');
          output.textContent = 'Error: Failed to process file. Ensure it’s a valid text file.';
        }
      };
      reader.onerror = function() {
        output.classList.add('error');
        output.classList.remove('loading');
        output.textContent = 'Error: Could not read the file.';
      };
      reader.readAsText(fileInput.files[0]);
    }
  </script>
</body>
</html>
