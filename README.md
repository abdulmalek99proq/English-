<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AI English Tutor</title>
  <style>
    body { background: #f0f2f5; font-family: sans-serif; margin: 0; padding: 0; }
    .chat-container { max-width: 600px; margin: 40px auto; background: white; border-radius: 10px; box-shadow: 0 0 10px rgba(0,0,0,0.1); overflow: hidden; }
    .chat-header { background: #007bff; color: white; padding: 16px; font-size: 20px; }
    .chat-body { height: 500px; overflow-y: auto; padding: 16px; display: flex; flex-direction: column; }
    .message { margin-bottom: 10px; }
    .user { align-self: flex-end; background: #d1e7dd; padding: 10px; border-radius: 10px; max-width: 80%; }
    .bot { align-self: flex-start; background: #f8d7da; padding: 10px; border-radius: 10px; max-width: 80%; }
    .chat-input { display: flex; border-top: 1px solid #ccc; }
    .chat-input input { flex: 1; padding: 14px; border: none; font-size: 16px; }
    .chat-input button { padding: 14px; border: none; background: #007bff; color: white; font-size: 16px; cursor: pointer; }
  </style>
</head>
<body>
  <div class="chat-container">
    <div class="chat-header">AI English Tutor</div>
    <div class="chat-body" id="chat"></div>
    <div class="chat-input">
      <input type="text" id="input" placeholder="Type your message..." onkeydown="if(event.key === 'Enter') sendMessage()"/>
      <button onclick="sendMessage()">Send</button>
    </div>
  </div>  <script>
    const chat = document.getElementById('chat');

    function appendMessage(role, text) {
      const msg = document.createElement('div');
      msg.className = 'message ' + (role === 'user' ? 'user' : 'bot');
      msg.textContent = text;
      chat.appendChild(msg);
      chat.scrollTop = chat.scrollHeight;
    }

    async function sendMessage() {
      const input = document.getElementById('input');
      const userText = input.value.trim();
      if (!userText) return;
      appendMessage('user', userText);
      input.value = '';

      const res = await fetch('https://api.openai.com/v1/chat/completions', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer YOUR_OPENAI_API_KEY'
        },
        body: JSON.stringify({
          model: 'gpt-4',
          messages: [
            { role: 'system', content: 'You are an English tutor. Correct grammar, explain clearly, and guide the learner in a helpful way.' },
            { role: 'user', content: userText }
          ]
        })
      });

      const data = await res.json();
      const botReply = data.choices[0].message.content;
      appendMessage('bot', botReply);
    }
  </script></body>
</html>
