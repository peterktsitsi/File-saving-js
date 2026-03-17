# File saving with js
a simple single page html file with style and js, for saving a file on your local drive

# 📄 Js File Writer Demo

This app lets users create and save a `.txt` file using two browser-safe methods.

---

## 🧠 Core Flow

1. Get user input (filename + content)
2. Create file data in memory
3. Save using:
   - Download (all browsers)
   - File Picker API (Chrome/Edge)

---

## ⚙️ 1. Feature Detection

```js
if ('showSaveFilePicker' in window) {
  document.getElementById('saveBtn').classList.remove('hidden');
}
```

- Checks if advanced file API is supported
- Shows button only when available

## 📥 2. Get Inputs

```js
function getInputs() {
  const name = document.getElementById('filename').value.trim() || 'my-file';
  const content = document.getElementById('content').value;
  return { name, content };
}
```

- Reads user input
- Adds default filename

## 🔔 3. UI Feedback

```js
function showNotice(msg, type) {
  const el = document.getElementById('notice');
  el.textContent = msg;
  el.className = 'notice show ' + type;
}
```
- Displays success/error messages

## ⬇️ 4. Download Method (All Browsers)

```js
function downloadFile() {
  const { name, content } = getInputs();

  const blob = new Blob([content], { type: 'text/plain' });
  const url = URL.createObjectURL(blob);

  const a = document.createElement('a');
  a.href = url;
  a.download = name + '.txt';
  a.click();

  URL.revokeObjectURL(url);
}
```
Key Idea:
- Create file in memory > trigger download via <a>

## 💾 5. File System API (Chrome/Edge)

```js
async function saveWithPicker() {
  const { name, content } = getInputs();

  const fileHandle = await window.showSaveFilePicker({
    suggestedName: name + '.txt'
  });

  const writable = await fileHandle.createWritable();
  await writable.write(content);
  await writable.close();
}```

Key Idea:

Opens real save dialog
Writes file directly to disk (with permission)
