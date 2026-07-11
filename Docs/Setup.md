# SolSmartBrowser – Setup Guide

Follow this guide to set up your own modern browser app for Samsung Smart TV or Smart Monitor.

---

## 📦 Prerequisites

- A computer with internet access.
- A free [Netlify](https://www.netlify.com/) account (email‑only signup, no captcha).
- Your Samsung Smart Monitor M7 (or any TV with a web browser).

---

## 1. Create the Browser App File

Create a folder on your PC, for example `SolSmartBrowser`, and inside it create a new file named `index.html`.

Copy the following complete browser code into that file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Modern Browser</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      background: #0a0a0a;
      font-family: 'Segoe UI', Roboto, system-ui, sans-serif;
      display: flex;
      flex-direction: column;
      height: 100vh;
      overflow: hidden;
    }
    .toolbar {
      background: #1a1a2e;
      padding: 12px 16px;
      display: flex;
      align-items: center;
      gap: 12px;
      border-bottom: 1px solid #2a2a4a;
      flex-shrink: 0;
    }
    .nav-btn {
      background: #2a2a4a;
      border: none;
      color: #e0e0ff;
      width: 48px;
      height: 48px;
      border-radius: 12px;
      font-size: 24px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 0.2s;
    }
    .nav-btn:hover, .nav-btn:active { background: #3a3a6a; }
    #url-bar {
      flex: 1;
      background: #12121e;
      border: 1px solid #3a3a6a;
      border-radius: 24px;
      padding: 12px 20px;
      color: #e0e0ff;
      font-size: 20px;
      outline: none;
    }
    #url-bar:focus { border-color: #7a7aff; }
    .go-btn {
      background: #7a7aff;
      color: white;
      border: none;
      padding: 12px 24px;
      border-radius: 24px;
      font-size: 20px;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.2s;
    }
    .go-btn:hover { background: #5a5aea; }
    .bookmarks {
      background: #12121e;
      padding: 8px 16px;
      display: flex;
      gap: 12px;
      border-bottom: 1px solid #2a2a4a;
      flex-shrink: 0;
      overflow-x: auto;
    }
    .bookmark {
      background: #2a2a4a;
      color: #ccc;
      padding: 8px 18px;
      border-radius: 20px;
      font-size: 18px;
      white-space: nowrap;
      cursor: pointer;
      transition: background 0.2s;
    }
    .bookmark:hover, .bookmark:active { background: #3a3a6a; color: white; }
    .browser-view {
      flex: 1;
      display: flex;
      background: #050510;
    }
    iframe {
      width: 100%;
      height: 100%;
      border: none;
    }
  </style>
</head>
<body>
  <div class="toolbar">
    <button class="nav-btn" onclick="goBack()" title="Back">←</button>
    <button class="nav-btn" onclick="goForward()" title="Forward">→</button>
    <button class="nav-btn" onclick="reload()" title="Refresh">⟳</button>
    <input type="text" id="url-bar" placeholder="Enter URL or search..." autofocus>
    <button class="go-btn" onclick="navigate()">Go</button>
  </div>

  <div class="bookmarks">
    <span class="bookmark" onclick="loadURL('https://www.google.com')">Google</span>
    <span class="bookmark" onclick="loadURL('https://www.youtube.com/tv')">YouTube TV</span>
    <span class="bookmark" onclick="loadURL('https://www.netflix.com')">Netflix</span>
    <span class="bookmark" onclick="loadURL('https://github.com')">GitHub</span>
    <span class="bookmark" onclick="loadURL('https://space-explorer-test.netlify.app')">Space Explorer</span>
  </div>

  <div class="browser-view">
    <iframe id="browser-frame" sandbox="allow-scripts allow-same-origin allow-forms allow-popups allow-popups-to-escape-sandbox"></iframe>
  </div>

  <script>
    const frame = document.getElementById('browser-frame');
    const urlBar = document.getElementById('url-bar');

    frame.src = 'https://www.google.com';

    frame.addEventListener('load', () => {
      try {
        if (frame.contentWindow.location.href) {
          urlBar.value = frame.contentWindow.location.href;
        }
      } catch (e) {}
    });

    function navigate() {
      let raw = urlBar.value.trim();
      if (!raw) return;
      if (!/^https?:\/\//i.test(raw)) {
        if (raw.includes('.') || raw.includes('localhost')) {
          raw = 'https://' + raw;
        } else {
          raw = 'https://www.google.com/search?q=' + encodeURIComponent(raw);
        }
      }
      loadURL(raw);
    }

    function loadURL(url) {
      urlBar.value = url;
      frame.src = url;
    }

    function goBack() {
      if (frame.contentWindow.history.length > 1) frame.contentWindow.history.back();
    }

    function goForward() {
      if (frame.contentWindow.history.length > 1) frame.contentWindow.history.forward();
    }

    function reload() {
      frame.contentWindow.location.reload();
    }

    urlBar.addEventListener('keydown', (e) => {
      if (e.key === 'Enter') navigate();
    });
  </script>
</body>
</html>
```

> **Note:** You can customise the bookmarks in the `<div class="bookmarks">` section.

---

## 2. Deploy with Netlify Drop

- Go to [app.netlify.com/drop](https://app.netlify.com/drop)
- **Drag and drop** your `SolSmartBrowser` folder (the one containing `index.html`) onto the browser window.
- Sign up with your email and a password (no captcha, no phone verification).
- Your site will be published instantly. You’ll see a unique URL like `random-name.netlify.app`.
- Click the URL to verify the browser app loads on your PC.

---

## 3. Use on Your Samsung Smart Monitor / TV

- On your TV, open the **built-in web browser** (usually labeled “Internet” or “Web” on the home bar).
- Navigate to the Netlify URL you just created.
- Once the page loads, press the **Tools** button on your remote (or the `…` key) and choose **Add to Home Screen** (if available) or simply bookmark the page.
- From now on, you can launch SolSmartBrowser with a single click.

The app will open full‑screen and function just like a native browser.

---

## 🎨 Customisation Tips

- **Change bookmarks:** Edit the `<span class="bookmark">` elements inside `index.html`.
- **Change colours:** Modify the CSS variables in the `<style>` block (e.g., `background`, `color`).
- **Add a virtual keyboard:** For easier typing via remote, you could embed a JavaScript virtual keyboard library (like [simple-keyboard](https://simple-keyboard.com/)).
- **Enable PWA:** Add a `manifest.json` to make it installable as a Progressive Web App (the TV browser may support it).

---

## ❓ Troubleshooting

**“Page not loading” on TV?**  
- Verify your Netlify URL is correct and the site is published (check on your phone/PC first).  
- Some TVs block mixed content; ensure your Netlify site is HTTPS (Netlify does this automatically).

**“Iframe not displaying a website”?**  
- Many major sites (Google, YouTube, etc.) block iframes. You can still navigate to them; they’ll open in a new tab on the TV’s browser (since the iframe sandbox allows popups). Just click the bookmark or URL – the TV will handle it.

**Want to update the app?**  
- Simply replace the `index.html` on your PC, drag the folder onto Netlify Drop again. It will overwrite the previous deployment and keep the same URL.

---

## 🔗 Back to README

Return to the main [README.md](../README.md) for an overview and project philosophy.