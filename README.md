<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
  <title>codeSite · learn coding for site editing</title>
  <style>
    /* clean, modern reset and layout */
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: 'Segoe UI', system-ui, -apple-system, Roboto, 'Helvetica Neue', sans-serif;
      background: #0f1117;
      height: 100vh;
      display: flex;
      flex-direction: column;
      color: #e2e8f0;
    }

    /* header with gentle gradient */
    .app-header {
      background: linear-gradient(105deg, #1e1e2f 0%, #1a1c2a 100%);
      border-bottom: 1px solid #2d3143;
      padding: 0.9rem 1.8rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 1rem;
      box-shadow: 0 4px 14px rgba(0,0,0,0.5);
    }

    .logo {
      display: flex;
      align-items: center;
      gap: 0.4rem;
      font-weight: 650;
      font-size: 1.5rem;
      letter-spacing: -0.3px;
      background: linear-gradient(to right, #b3c7ff, #8da4f0);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
    }

    .logo span {
      font-size: 1.8rem;
      margin-right: 0.2rem;
    }

    .controls {
      display: flex;
      align-items: center;
      gap: 1rem;
      flex-wrap: wrap;
    }

    .lang-tabs {
      display: flex;
      background: #212433;
      border-radius: 2rem;
      padding: 0.25rem;
      border: 1px solid #363b54;
    }

    .lang-btn {
      background: transparent;
      border: none;
      color: #b9c2e0;
      font-weight: 500;
      font-size: 0.9rem;
      padding: 0.45rem 1.2rem;
      border-radius: 2rem;
      cursor: pointer;
      transition: all 0.2s ease;
      letter-spacing: 0.3px;
      background: transparent;
    }

    .lang-btn.active {
      background: #3f4b7a;
      color: white;
      box-shadow: 0 4px 10px rgba(80, 100, 200, 0.3);
      font-weight: 600;
    }

    .run-btn {
      background: #5b6eae;
      border: none;
      color: white;
      font-weight: 600;
      font-size: 0.95rem;
      padding: 0.5rem 1.6rem;
      border-radius: 2rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 0.4rem;
      background: linear-gradient(135deg, #5f73c0, #3f4980);
      box-shadow: 0 6px 16px rgba(70, 90, 180, 0.4);
      transition: 0.2s;
      border: 1px solid #7b8ed0;
      letter-spacing: 0.4px;
    }

    .run-btn:hover {
      filter: brightness(1.15);
      transform: scale(1.02);
      box-shadow: 0 8px 18px rgba(90, 110, 220, 0.5);
    }

    .run-btn:active {
      transform: scale(0.97);
    }

    /* main layout: left coding panel | right preview */
    .main-container {
      display: flex;
      flex: 1;
      overflow: hidden;
      background: #0b0d14;
    }

    /* left editor panel */
    .editor-panel {
      width: 50%;
      display: flex;
      flex-direction: column;
      border-right: 2px solid #2a2e42;
      background: #12141f;
    }

    .editor-header {
      padding: 0.6rem 1.2rem;
      font-size: 0.85rem;
      font-weight: 500;
      color: #aab4e0;
      background: #181b28;
      border-bottom: 1px solid #2d3143;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      letter-spacing: 0.4px;
    }

    .code-area {
      flex: 1;
      display: flex;
      flex-direction: column;
    }

    textarea {
      flex: 1;
      background: #0d0f18;
      color: #e3e9ff;
      border: none;
      padding: 1.2rem;
      font-family: 'JetBrains Mono', 'Fira Code', 'Cascadia Code', monospace;
      font-size: 0.9rem;
      line-height: 1.6;
      resize: none;
      outline: none;
      caret-color: #9bb5ff;
      white-space: pre-wrap;
      tab-size: 2;
    }

    textarea::placeholder {
      color: #484e6b;
      font-style: italic;
    }

    /* right preview panel */
    .preview-panel {
      width: 50%;
      display: flex;
      flex-direction: column;
      background: #ffffff; /* white background for real site preview */
    }

    .preview-header {
      padding: 0.6rem 1.2rem;
      background: #1e2132;
      border-bottom: 1px solid #2d3143;
      display: flex;
      align-items: center;
      gap: 0.5rem;
      color: #cbd5ff;
      font-size: 0.85rem;
      font-weight: 500;
    }

    .preview-frame-wrapper {
      flex: 1;
      background: #f5f7fc;
      padding: 0;
      margin: 0;
    }

    iframe {
      width: 100%;
      height: 100%;
      border: none;
      background: white;
    }

    /* status indicator */
    .status-dot {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: #5bc67b;
      display: inline-block;
      margin-right: 0.4rem;
    }

    @media (max-width: 700px) {
      .main-container {
        flex-direction: column;
      }
      .editor-panel, .preview-panel {
        width: 100%;
        height: 50%;
      }
      .app-header {
        padding: 0.7rem 1rem;
      }
    }
  </style>
</head>
<body>
  <header class="app-header">
    <div class="logo">
      <span>✦</span> codeSite
    </div>
    <div class="controls">
      <div class="lang-tabs" id="languageTabs">
        <button class="lang-btn active" data-lang="html">HTML</button>
        <button class="lang-btn" data-lang="css">CSS</button>
        <button class="lang-btn" data-lang="js">JS</button>
      </div>
      <button class="run-btn" id="runButton">
        <span>▶</span> Run & Preview
      </button>
    </div>
  </header>

  <div class="main-container">
    <!-- left side: coding editors -->
    <div class="editor-panel">
      <div class="editor-header">
        <span class="status-dot"></span> <span id="activeEditorLabel">HTML</span> editor
      </div>
      <div class="code-area">
        <textarea id="htmlEditor" placeholder="Write your HTML here..."><!-- HTML structure -->
<h1>👋 Hello, site editor!</h1>
<p>Edit this <strong>HTML</strong>, then switch tabs to add CSS or JavaScript.</p>
<button id="demoBtn">Click me for JS magic</button>
<div class="card">
  <p>✨ Your live preview appears on the right.</p>
</div></textarea>
        <textarea id="cssEditor" placeholder="/* CSS styles */" style="display: none;">body {
  font-family: 'Segoe UI', Roboto, system-ui;
  background: #f0f4ff;
  margin: 2rem;
  color: #1e293b;
}
h1 {
  color: #2c3e7b;
  border-bottom: 3px solid #b7c4ff;
  padding-bottom: 0.4rem;
}
.card {
  background: white;
  border-radius: 1.5rem;
  padding: 1.5rem;
  margin-top: 1.2rem;
  box-shadow: 0 12px 28px rgba(0,0,0,0.08);
  max-width: 400px;
}
button {
  background: #4f5db0;
  color: white;
  border: none;
  padding: 0.7rem 1.4rem;
  border-radius: 2rem;
  font-weight: bold;
  cursor: pointer;
  margin: 0.8rem 0;
  transition: 0.2s;
}
button:hover {
  background: #33408a;
}
</textarea>
        <textarea id="jsEditor" placeholder="// JavaScript code" style="display: none;">// JavaScript interacts with the HTML
document.getElementById('demoBtn').addEventListener('click', () => {
  alert('🎉 JavaScript is working! You edited the site live.');
  const card = document.querySelector('.card');
  if (card) {
    card.style.background = '#eef2ff';
    card.innerHTML += '<p style="color:green;">✅ JS modified this card!</p>';
  }
});
</textarea>
      </div>
    </div>

    <!-- right side: live site preview -->
    <div class="preview-panel">
      <div class="preview-header">
        <span>🌐</span> Live site preview
      </div>
      <div class="preview-frame-wrapper">
        <iframe id="previewFrame" title="live-coding-preview" sandbox="allow-scripts allow-same-origin"></iframe>
      </div>
    </div>
  </div>

  <script>
    (function() {
      // ----- DOM elements -----
      const htmlEditor = document.getElementById('htmlEditor');
      const cssEditor = document.getElementById('cssEditor');
      const jsEditor = document.getElementById('jsEditor');
      const previewFrame = document.getElementById('previewFrame');
      const runButton = document.getElementById('runButton');
      const langButtons = document.querySelectorAll('.lang-btn');
      const activeEditorLabel = document.getElementById('activeEditorLabel');

      // Current active language
      let currentLang = 'html';

      // ----- Switch editor tabs -----
      function switchEditor(lang) {
        // Hide all editors
        htmlEditor.style.display = 'none';
        cssEditor.style.display = 'none';
        jsEditor.style.display = 'none';

        // Show selected
        if (lang === 'html') {
          htmlEditor.style.display = 'block';
          activeEditorLabel.textContent = 'HTML';
        } else if (lang === 'css') {
          cssEditor.style.display = 'block';
          activeEditorLabel.textContent = 'CSS';
        } else if (lang === 'js') {
          jsEditor.style.display = 'block';
          activeEditorLabel.textContent = 'JavaScript';
        }

        // Update active button style
        langButtons.forEach(btn => {
          const btnLang = btn.getAttribute('data-lang');
          if (btnLang === lang) {
            btn.classList.add('active');
          } else {
            btn.classList.remove('active');
          }
        });

        currentLang = lang;
        // Focus on the visible editor for better UX
        const activeTextarea = document.querySelector(`#${lang}Editor`);
        if (activeTextarea) {
          activeTextarea.focus();
        }
      }

      // Add click listeners to language tabs
      langButtons.forEach(btn => {
        btn.addEventListener('click', (e) => {
          const lang = e.currentTarget.getAttribute('data-lang');
          switchEditor(lang);
        });
      });

      // ----- Build complete document and update preview -----
      function updatePreview() {
        const htmlContent = htmlEditor.value;
        const cssContent = cssEditor.value;
        const jsContent = jsEditor.value;

        // Construct a full HTML document that includes user code
        const fullDocument = `<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>${cssContent}</style>
</head>
<body>
  ${htmlContent}
  <script>${jsContent}<\/script>
</body>
</html>`;

        // Write into iframe using srcdoc-like approach via blob or directly
        try {
          // Using srcdoc would be cleaner but we ensure cross-origin safety.
          // We overwrite iframe content.
          const iframeDoc = previewFrame.contentDocument || previewFrame.contentWindow?.document;
          if (iframeDoc) {
            iframeDoc.open();
            iframeDoc.write(fullDocument);
            iframeDoc.close();
          } else {
            // Fallback: use srcdoc via setting attribute (might not work in old browsers)
            // But we prefer contentDocument approach.
            console.warn('Using fallback srcdoc');
            previewFrame.srcdoc = fullDocument;
          }
        } catch (e) {
          console.warn('iframe direct write failed, using srcdoc', e);
          previewFrame.srcdoc = fullDocument;
        }
      }

      // Initial preview load
      updatePreview();

      // Run button triggers update
      runButton.addEventListener('click', () => {
        updatePreview();
        // subtle feedback
        runButton.style.transform = 'scale(0.96)';
        setTimeout(() => {
          runButton.style.transform = '';
        }, 120);
      });

      // Optional: auto-update on input? We'll keep manual "Run" for learning control.
      // But we can also listen for Ctrl+Enter or shortcut
      document.addEventListener('keydown', (e) => {
        // Ctrl+Enter or Cmd+Enter triggers run
        if ((e.ctrlKey || e.metaKey) && e.key === 'Enter') {
          e.preventDefault();
          updatePreview();
          // brief flash effect on run button
          runButton.style.background = 'linear-gradient(135deg, #7b8ed0, #4f5a9c)';
          setTimeout(() => {
            runButton.style.background = 'linear-gradient(135deg, #5f73c0, #3f4980)';
          }, 150);
        }
      });

      // Also allow switching tabs with keyboard? not necessary but nice.
      // Ensure first tab active style correct on load
      switchEditor('html');

      // Handle window resize for iframe stability
      window.addEventListener('resize', () => {
        // no action needed, iframe adapts
      });

      // Small feature: if user edits and clicks run, update.
      // Also for better learning, show initial preview with default content.

      // Make sure the preview runs after everything is ready
      window.addEventListener('load', () => {
        updatePreview();
      });

    })();
  </script>
</body>
</html>
