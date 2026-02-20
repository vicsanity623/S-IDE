<div align="center">

# 🛠️ SandboxIDE
**The Zero-Config, Single-File Browser Workstation**

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)]()
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)]()
[![CodeMirror](https://img.shields.io/badge/CodeMirror-5.65-blue?style=for-the-badge)]()
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-0-success?style=for-the-badge)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

A lightweight, fully functional Web IDE contained entirely within a **single `.html` file**. Write code, manage virtual files, and preview your web applications with a built-in proxied developer console—all without starting a local server.

[Features](#-features) • [Quick Start](#-quick-start) • [How to Use](#%EF%B8%8F-how-to-use) • [Under the Hood](#-under-the-hood) • [Contributing](#-contributing)

</div>

---

## ⚡ Features

- **📦 Zero-Server Architecture**: Runs purely in the browser. No Node.js, no Webpack, no local servers required.
- **🗂️ Virtual File System**: Create, edit, and link multiple HTML, CSS, and JS files just like a real local project.
- **🔄 On-the-Fly Bundler**: Automatically intercepts `<script src="...">` and `<link href="...">` tags and injects your virtual files dynamically before rendering.
- **🖥️ Integrated Smart Console**: Intercepts standard `console.log` calls from the preview and displays them in the IDE. Includes custom serialization for complex DOM elements (like `<canvas>`) and Web APIs (like `CanvasRenderingContext2D`).
- **🎨 SOTA Editor Experience**: Powered by CodeMirror with the Dracula theme, offering syntax highlighting, line numbers, and auto-indentation.

---

## 🚀 Quick Start

Because SandboxIDE is completely standalone, installation takes seconds:

1. **Download** or copy the `ide.html` file to your local machine.
2. **Double-click** `ide.html` to open it in Chrome, Firefox, Safari, or Edge.
3. **Start Coding!** 

*(No internet connection is required after the initial load of the CodeMirror CDN assets).*

---

## 🕹️ How to Use

### 1. Managing Files
On the left-hand side is your **File Explorer**.
- Click the **`+`** button in the sidebar to create a new file (e.g., `style.css` or `app.js`).
- Click on any file name to open it in the editor.

### 2. Linking Files Together
SandboxIDE acts exactly like a real web server. You can link the files you create in your virtual file system directly inside your `index.html` file.

**Example `index.html`:**
```html
<!DOCTYPE html>
<html>
<head>
    <!-- SandboxIDE will bundle this automatically! -->
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <canvas id="game"></canvas>
    
    <!-- SandboxIDE will bundle this automatically! -->
    <script src="index.js"></script>
</body>
</html>
```

### 3. Using the Interactive Console
Unlike standard iframes that hide logs in your browser's F12 dev tools, SandboxIDE features a built-in console panel.

**Example `index.js`:**
```javascript
// 1. Grab the canvas
const game = document.getElementById("game");
console.log("Canvas Element:", game);

// 2. Set dimensions
game.width = 800;
game.height = 800;

// 3. Get the context
const ctx = game.getContext("2d");

// 4. SandboxIDE natively serializes Complex Objects so you don't get "[object Object]" errors!
console.log("Canvas Context:", ctx);

ctx.fillStyle = "blue";
ctx.fillRect(50, 50, 100, 100);
```

### 4. Running Your Code
Hit the **`▶ Run`** button in the top right. 
SandboxIDE will:
1. Clear the previous console logs.
2. Read your `index.html`.
3. Search for external scripts and styles.
4. Replace them with the code you wrote in your virtual file system.
5. Render the result instantly in the Live Preview panel!

---

## 🧠 Under the Hood

Curious how a multi-file IDE works in a single HTML file? Here is the magic:

<details>
<summary><strong>1. The Virtual File System (VFS)</strong></summary>
<br>
All files are stored in a simple JavaScript Dictionary object (`const files = {}`). As you switch tabs, CodeMirror's value is swapped out and saved in real-time.
</details>

<details>
<summary><strong>2. RegEx Bundling</strong></summary>
<br>
When you click Run, the app uses Regular Expressions to parse your HTML string. It looks for `<script src="[filename]"></script>`. If `[filename]` exists in the VFS dictionary, it replaces the network request with a literal, inline `<script>` tag containing your exact code.
</details>

<details>
<summary><strong>3. Iframe Console Proxying</strong></summary>
<br>
Before injecting the code into the `<iframe>` via `srcdoc`, SandboxIDE prepends an Interceptor Script. This script hijacks `console.log`, `warn`, `error`, and `window.onerror`. It intercepts the arguments, serializes them (handling Edge-Cases like DOM elements and Contexts), and uses `window.parent.postMessage` to send the data back to the main IDE UI.
</details>

---

## 🛣️ Roadmap

- [x] Virtual File System creation
- [x] Syntax Highlighting via CodeMirror
- [x] Iframe Live Preview
- [x] Smart Console Output
- [ ] Auto-save to `localStorage` (so you don't lose code on refresh)
- [ ] Drag-and-drop file imports
- [ ] Support for Markdown rendering
- [ ] Export project as a `.zip` file

---

## 🤝 Contributing

Contributions make the open-source community an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.

---

<div align="center">
  <b>Built with ❤️ for rapid browser-based prototyping.</b>
</div>
