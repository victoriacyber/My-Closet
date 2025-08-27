<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My Cute Closet 💖</title>
  <style>
    body {
      font-family: 'Poppins', sans-serif;
      margin: 0;
      background: linear-gradient(135deg, #ffd6e0, #ffeaf4);
      color: #444;
      text-align: center;
    }
    h1 {
      font-size: 2.5em;
      margin: 20px;
      color: #e75480;
    }
    .upload-box {
      margin: 20px auto;
      padding: 20px;
      background: #fff0f6;
      border: 2px dashed #e75480;
      border-radius: 20px;
      width: 300px;
    }
    input[type="file"] {
      margin-top: 10px;
    }
    .catalogue {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      margin: 20px;
      gap: 15px;
    }
    .item {
      width: 120px;
      height: 120px;
      background: white;
      border-radius: 20px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      overflow: hidden;
      cursor: grab;
      transition: transform 0.2s;
    }
    .item:hover {
      transform: scale(1.05);
    }
    .item img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    .outfit-area {
      margin: 30px auto;
      padding: 20px;
      background: #fff0f6;
      border-radius: 30px;
      width: 80%;
    }
    .outfit-slots {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 15px;
    }
    .slot {
      width: 120px;
      height: 120px;
      border: 2px dashed #e75480;
      border-radius: 20px;
      background: #ffeaf4;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 12px;
      color: #999;
    }
    .slot img {
      width: 100%;
      height: 100%;
      border-radius: 20px;
      object-fit: cover;
    }
    button {
      margin: 15px;
      padding: 10px 20px;
      border: none;
      border-radius: 25px;
      background: #e75480;
      color: white;
      font-size: 1em;
      cursor: pointer;
      transition: background 0.3s;
    }
    button:hover {
      background: #ff8fa3;
    }
  </style>
</head>
<body>
  <h1>💖 My Cute Closet 💖</h1>

  <div class="upload-box">
    <p>Upload your clothes 👗</p>
    <input type="file" id="fileInput" accept="image/*" multiple>
  </div>

  <h2>🌸 Catalogue 🌸</h2>
  <div class="catalogue" id="catalogue"></div>

  <div class="outfit-area">
    <h2>✨ Outfit Builder ✨</h2>
    <div class="outfit-slots">
      <div class="slot" ondrop="drop(event)" ondragover="allowDrop(event)">Top</div>
      <div class="slot" ondrop="drop(event)" ondragover="allowDrop(event)">Bottom</div>
      <div class="slot" ondrop="drop(event)" ondragover="allowDrop(event)">Shoes</div>
    </div>
    <button onclick="saveOutfit()">💌 Save Outfit</button>
    <button onclick="loadOutfits()">📂 Load Outfits</button>
  </div>

  <script>
    const fileInput = document.getElementById('fileInput');
    const catalogue = document.getElementById('catalogue');
    let outfits = JSON.parse(localStorage.getItem('outfits') || '[]');

    fileInput.addEventListener('change', () => {
      [...fileInput.files].forEach(file => {
        const reader = new FileReader();
        reader.onload = (e) => {
          const div = document.createElement('div');
          div.className = 'item';
          div.draggable = true;
          div.ondragstart = drag;
          const img = document.createElement('img');
          img.src = e.target.result;
          div.appendChild(img);
          catalogue.appendChild(div);
        };
        reader.readAsDataURL(file);
      });
    });

    function allowDrop(ev) {
      ev.preventDefault();
    }
    function drag(ev) {
      ev.dataTransfer.setData("text", ev.target.src);
    }
    function drop(ev) {
      ev.preventDefault();
      const data = ev.dataTransfer.getData("text");
      ev.target.innerHTML = `<img src="${data}">`;
    }

    function saveOutfit() {
      const slots = document.querySelectorAll('.slot img');
      const outfit = [];
      slots.forEach(s => outfit.push(s ? s.src : null));
      outfits.push(outfit);
      localStorage.setItem('outfits', JSON.stringify(outfits));
      alert("Outfit saved! 💖");
    }

    function loadOutfits() {
      if (outfits.length === 0) {
        alert("No outfits saved yet 🌸");
        return;
      }
      const lastOutfit = outfits[outfits.length - 1];
      document.querySelectorAll('.slot').forEach((slot, i) => {
        slot.innerHTML = lastOutfit[i] ? `<img src="${lastOutfit[i]}">` : slot.innerText;
      });
    }
  </script>
</body>
</html>
