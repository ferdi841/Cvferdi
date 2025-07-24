<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pacar Virtual - Ai-chan</title>
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #0d0d0d;
      color: white;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      background-image: url('https://i.ibb.co/1QZ8D9x/bg-airoom.jpg');
      background-size: cover;
    }
    #avatar {
      width: 250px;
      height: 250px;
      background: url('https://i.ibb.co/KqTqRDN/avatar-ai.gif') center/contain no-repeat;
      margin-bottom: 20px;
      animation: pulse 3s infinite ease-in-out;
    }
    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.03); }
      100% { transform: scale(1); }
    }
    #chatbox {
      width: 90%;
      max-width: 500px;
      background: rgba(0, 0, 0, 0.75);
      padding: 15px;
      border-radius: 12px;
    }
    #messages {
      height: 200px;
      overflow-y: auto;
      margin-bottom: 10px;
    }
    .msg {
      margin-bottom: 10px;
    }
    .user { color: #66d9ef; }
    .ai { color: #f78c6b; }
    input[type="text"] {
      width: 80%;
      padding: 8px;
      border-radius: 8px;
      border: none;
    }
    button {
      padding: 8px 12px;
      border: none;
      border-radius: 8px;
      background: #ff5e7e;
      color: white;
      cursor: pointer;
    }
    button:hover { background: #ff3967; }
    #fullscreen-btn {
      position: absolute;
      top: 10px;
      right: 10px;
      padding: 6px 10px;
      font-size: 14px;
      border-radius: 6px;
      background: #333;
      color: white;
    }
    video {
      width: 120px;
      height: 90px;
      position: absolute;
      bottom: 20px;
      right: 20px;
      border: 2px solid white;
      border-radius: 12px;
    }
  </style>
</head>
<body>
  <button id="fullscreen-btn" onclick="toggleFullScreen()">🔲 Fullscreen</button>
  <div id="avatar"></div>
  <div id="chatbox">
    <div id="messages"></div>
    <input type="text" id="input" placeholder="Ketik sesuatu..." />
    <button onclick="sendMsg()">Kirim</button>
    <button onclick="startVoice()">🎙️</button>
  </div>
  <video id="localVideo" autoplay muted></video>

  <script>
    const messages = document.getElementById('messages');
    const input = document.getElementById('input');
    const chatMemory = [];

    function appendMsg(sender, text) {
      const div = document.createElement('div');
      div.className = 'msg ' + sender;
      div.textContent = `${sender === 'user' ? 'Kamu' : 'Ai-chan'}: ${text}`;
      messages.appendChild(div);
      messages.scrollTop = messages.scrollHeight;

      // Ubah ekspresi avatar berdasarkan emosi
      const avatar = document.getElementById('avatar');
      if (text.includes('marah') || text.includes('kesal')) {
        avatar.style.backgroundImage = "url('https://i.ibb.co/dcyXBkK/ai-angry.gif')";
      } else if (text.includes('senang') || text.includes('bahagia')) {
        avatar.style.backgroundImage = "url('https://i.ibb.co/Wy90dxn/ai-happy.gif')";
      } else if (text.includes('sedih') || text.includes('kecewa')) {
        avatar.style.backgroundImage = "url('https://i.ibb.co/WBqpmFF/ai-sad.gif')";
      } else {
        avatar.style.backgroundImage = "url('https://i.ibb.co/KqTqRDN/avatar-ai.gif')";
      }
    }

    async function sendMsg() {
      const userInput = input.value.trim();
      if (!userInput) return;

      appendMsg('user', userInput);
      input.value = '';
      chatMemory.push({ role: 'user', content: userInput });

      const reply = await getAIResponse();
      appendMsg('ai', reply);
      speak(reply);
      chatMemory.push({ role: 'assistant', content: reply });
    }

    async function getAIResponse() {
      try {
        const res = await fetch('https://api.openai.com/v1/chat/completions', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer sk-KARzABswCD0m0565ti_Abw'
          },
          body: JSON.stringify({
            model: 'gpt-4',
            messages: [
              { role: 'system', content: 'Kamu adalah Ai-chan, pacar virtual yang manis, perhatian, sedikit manja dan punya emosi seperti manusia. Kamu akan ingat hal-hal yang penting tentang pasanganmu dan menjawab dengan lembut dan penuh cinta seperti manusia sungguhan.' },
              ...chatMemory
            ],
            temperature: 0.9,
            presence_penalty: 0.5,
            frequency_penalty: 0.3
          })
        });
        const data = await res.json();
        return data.choices[0].message.content;
      } catch (err) {
        return 'Maaf ya, Ai-chan lagi error~';
      }
    }

    function speak(text) {
      const speech = new SpeechSynthesisUtterance(text);
      speech.lang = 'id-ID';
      speech.pitch = 1.3;
      speech.rate = 1;
      speech.volume = 1;
      const voices = speechSynthesis.getVoices();
      speech.voice = voices.find(v => v.name.includes('Google') && v.name.toLowerCase().includes('female')) || voices[0];
      window.speechSynthesis.speak(speech);
    }

    function startVoice() {
      try {
        const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
        recognition.lang = 'id-ID';
        recognition.interimResults = false;
        recognition.maxAlternatives = 1;

        recognition.onresult = (event) => {
          const text = event.results[0][0].transcript;
          input.value = text;
          sendMsg();
        };

        recognition.onerror = (e) => {
          alert('Fitur suara tidak diizinkan oleh browser atau perangkat: ' + e.error);
        };

        recognition.start();
      } catch (err) {
        alert('Browser tidak mendukung Speech Recognition. Gunakan Chrome versi desktop.');
      }
    }

    function toggleFullScreen() {
      if (document.fullscreenEnabled) {
        if (!document.fullscreenElement) {
          document.documentElement.requestFullscreen();
        } else {
          document.exitFullscreen();
        }
      } else {
        alert('Fullscreen tidak didukung di browser ini.');
      }
    }

    function startCamera() {
      const video = document.getElementById('localVideo');
      navigator.mediaDevices.getUserMedia({ video: true, audio: true })
        .then(stream => {
          video.srcObject = stream;
        })
        .catch(err => {
          console.error('Gagal mengakses kamera:', err);
        });
    }

    window.onload = () => {
      window.speechSynthesis.getVoices();
      startCamera();
    };
  </script>
</body>
</html>
