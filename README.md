# customer-chat-bot-model
A customer chatbot model is an AI tool that engages with users through conversation to provide assistance, answer questions, and enhance customer service experiences.

## customer chatbot support
To build a customer support model interface using Python, we can use the following steps:

1. **HTML**: The structure of the user interface.
2. **CSS**: Styling the interface to make it user-friendly.
3. **JavaScript (JS)**: Handling user interactions and fetching responses from the backend.
4. **Python**: For backend logic (i.e., customer support model), which can be implemented using a framework like Flask.

I'll guide you through creating the **HTML, CSS, and JS** part of the user interface first, then I'll explain how it can connect to a Python-based backend.

### 1. HTML - Structure of the UI

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Customer Support</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="chat-container">
        <div class="chat-box" id="chat-box">
            <!-- Chat messages will be appended here dynamically -->
        </div>
        <div class="input-area">
            <input type="text" id="user-input" placeholder="Type your message here..." autocomplete="off">
            <button id="send-button">Send</button>
        </div>
    </div>
    
    <script src="script.js"></script>
</body>
</html>
```

### 2. CSS - Styling the UI (styles.css)

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f9;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.chat-container {
    width: 400px;
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 10px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
}

.chat-box {
    height: 400px;
    padding: 15px;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 10px;
    border-bottom: 1px solid #ddd;
}

.message {
    padding: 10px;
    border-radius: 8px;
    width: fit-content;
    max-width: 80%;
}

.user-message {
    align-self: flex-end;
    background-color: #c8e6c9;
}

.bot-message {
    align-self: flex-start;
    background-color: #eee;
}

.input-area {
    display: flex;
    padding: 10px;
    background-color: #f9f9f9;
}

input[type="text"] {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
    margin-right: 10px;
}

button {
    padding: 10px 15px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #45a049;
}
```

### 3. JavaScript - Handling User Interaction (script.js)

```javascript
document.getElementById('send-button').addEventListener('click', function() {
    sendMessage();
});

document.getElementById('user-input').addEventListener('keypress', function(e) {
    if (e.key === 'Enter') {
        sendMessage();
    }
});

function sendMessage() {
    const userInput = document.getElementById('user-input').value.trim();
    
    if (userInput) {
        // Append the user message to the chat
        appendMessage(userInput, 'user');
        
        // Clear the input
        document.getElementById('user-input').value = '';
        
        // Send user message to the Python backend
        fetch('/get-response', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ message: userInput })
        })
        .then(response => response.json())
        .then(data => {
            // Append the bot response to the chat
            appendMessage(data.response, 'bot');
        })
        .catch(error => {
            console.error('Error:', error);
        });
    }
}

function appendMessage(text, sender) {
    const messageContainer = document.createElement('div');
    messageContainer.classList.add('message');
    
    if (sender === 'user') {
        messageContainer.classList.add('user-message');
    } else {
        messageContainer.classList.add('bot-message');
    }
    
    messageContainer.textContent = text;
    document.getElementById('chat-box').appendChild(messageContainer);
    
    // Scroll to the bottom of the chat
    document.getElementById('chat-box').scrollTop = document.getElementById('chat-box').scrollHeight;
}
```

### 4. Python (Backend using Flask)

Now, for the Python backend, we will use **Flask** to handle the `/get-response` route, which will take the user message and return a response. For this example, let's simulate a basic response from a customer support bot.

#### Python Backend (app.py)

```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Simulating a basic customer support bot
def get_bot_response(user_message):
    # This could be replaced with a more advanced AI model or lookup system
    if "hello" in user_message.lower():
        return "Hi there! How can I assist you today?"
    elif "issue" in user_message.lower():
        return "I'm sorry to hear you're having issues. Could you provide more details?"
    else:
        return "I'm not sure I understand. Can you please clarify?"

@app.route('/get-response', methods=['POST'])
def get_response():
    data = request.json
    user_message = data.get('message', '')
    
    bot_response = get_bot_response(user_message)
    
    return jsonify({"response": bot_response})

if __name__ == '__main__':
    app.run(debug=True)
```

### Explanation:
1. **HTML** defines the structure of the interface, with an input field and a button for users to interact with the customer support model.
2. **CSS** styles the chat interface to look user-friendly.
3. **JavaScript** handles sending the user's input to the backend (via the `/get-response` API) and displaying both the user’s messages and the bot's responses.
4. **Python/Flask** acts as the backend to process user messages and return appropriate responses.

### How to Run:
1. Install Flask:
    ```bash
    pip install Flask
    ```
2. Save the **Python** code to `app.py` and run it using:
    ```bash
    python app.py
    ```
3. Host the HTML, CSS, and JS files in the same directory and open the HTML in a browser. The Flask backend will handle user requests and provide responses dynamically.

This is a simple prototype, and you can expand it with more complex logic or integrate a real machine learning model.
