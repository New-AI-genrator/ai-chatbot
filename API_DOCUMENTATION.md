# NexusAI Chatbot - API Documentation

## Table of Contents

1. [Project Overview](#project-overview)
2. [Core Components](#core-components)
3. [JavaScript Functions](#javascript-functions)
4. [CSS Classes and Styling](#css-classes-and-styling)
5. [HTML Structure](#html-structure)
6. [Event Handlers](#event-handlers)
7. [Local Storage API](#local-storage-api)
8. [External API Integration](#external-api-integration)
9. [Usage Examples](#usage-examples)
10. [Configuration](#configuration)

## Project Overview

NexusAI is a modern, responsive AI chatbot web application featuring:
- Real-time chat interface
- Message history persistence
- Typing indicators
- Suggestion chips
- Dark mode support
- Responsive design

**File Structure:**
```
/
├── README.md
└── chat-bot.html (Main application file)
```

## Core Components

### 1. Chat Container
The main chat interface container that houses all chat functionality.

**HTML Structure:**
```html
<div class="chat-container">
    <div class="chat-header">...</div>
    <div class="chat-messages" id="chat-messages">...</div>
    <div class="chat-input-container">...</div>
</div>
```

**CSS Classes:**
- `.chat-container`: Main container with flex layout
- `.chat-header`: Header with gradient background
- `.chat-messages`: Scrollable message area
- `.chat-input-container`: Input area with form and suggestions

### 2. Message Component
Individual message bubbles for user and AI responses.

**HTML Structure:**
```html
<div class="message message-user|message-ai">
    <div class="avatar avatar-user|avatar-ai">
        <i class="fas fa-user|fa-robot"></i>
    </div>
    <div class="message-content message-content-user|message-content-ai">
        Message text
    </div>
</div>
```

**CSS Classes:**
- `.message`: Base message container
- `.message-user`: User message styling (right-aligned)
- `.message-ai`: AI message styling (left-aligned)
- `.avatar`: Circular avatar with icon
- `.message-content`: Message bubble with content

### 3. Typing Indicator
Animated dots showing AI is typing.

**HTML Structure:**
```html
<div class="typing-indicator" id="typing-indicator">
    <div class="typing-dots">
        <div class="typing-dot"></div>
        <div class="typing-dot"></div>
        <div class="typing-dot"></div>
    </div>
</div>
```

### 4. Suggestion Chips
Clickable suggestion buttons for quick queries.

**HTML Structure:**
```html
<div class="suggestions">
    <div class="suggestion-chip">Explain quantum computing</div>
    <div class="suggestion-chip">Write a poem about technology</div>
    <!-- More chips... -->
</div>
```

## JavaScript Functions

### Core Functions

#### `sendMessage()`
Sends a user message and triggers AI response.

**Usage:**
```javascript
sendMessage(); // Sends the current input value
```

**Functionality:**
- Validates input is not empty
- Adds user message to chat
- Clears input field
- Shows typing indicator
- Simulates AI response with delay

#### `addMessage(content, sender)`
Adds a new message to the chat interface.

**Parameters:**
- `content` (string): The message text
- `sender` (string): Either 'user' or 'ai'

**Usage:**
```javascript
addMessage("Hello, how are you?", "user");
addMessage("I'm doing well, thank you!", "ai");
```

**Functionality:**
- Creates message DOM elements
- Applies appropriate styling
- Inserts message before typing indicator
- Scrolls to bottom

#### `generateAIResponse(userMessage)`
Generates or fetches AI response to user message.

**Parameters:**
- `userMessage` (string): The user's input message

**Returns:**
- `string`: AI response text

**Usage:**
```javascript
const response = await generateAIResponse("What is AI?");
```

**Current Implementation:**
- Returns random response from predefined array
- Includes user message in response
- Fallback error handling

**API Integration Example:**
```javascript
async function generateAIResponse(userMessage) {
    try {
        const response = await fetch('YOUR_API_ENDPOINT', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ message: userMessage })
        });
        const data = await response.json();
        return data.reply;
    } catch (error) {
        console.error("API Error:", error);
        return "Sorry, I'm having trouble responding. Please try again.";
    }
}
```

### Utility Functions

#### `scrollToBottom()`
Scrolls the chat messages container to the bottom.

**Usage:**
```javascript
scrollToBottom();
```

#### `saveChatHistory()`
Saves current chat messages to localStorage.

**Usage:**
```javascript
saveChatHistory();
```

**Storage Format:**
```javascript
[
    { sender: "ai", content: "Hello! I'm NexusAI..." },
    { sender: "user", content: "Hi there!" },
    { sender: "ai", content: "How can I help you?" }
]
```

#### `loadChatHistory()`
Loads and displays saved chat history from localStorage.

**Usage:**
```javascript
loadChatHistory();
```

## CSS Classes and Styling

### Theme Variables
```css
:root {
    --primary: #6e48aa;
    --primary-light: #9d50bb;
    --gradient: linear-gradient(135deg, var(--primary), var(--primary-light));
    --bg-color: #f9f9ff;
    --card-bg: #ffffff;
    --text-dark: #2e2e48;
    --text-light: #7a7a9d;
    --user-bubble: var(--primary);
    --ai-bubble: #f1f3ff;
    --shadow: 0 10px 30px rgba(110, 72, 170, 0.1);
    --border-radius: 16px;
}
```

### Layout Classes

#### `.app-container`
Main application container with responsive design.

**Properties:**
- `max-width: 1200px`
- `margin: 0 auto`
- `padding: 2rem`
- `flex: 1`

#### `.chat-container`
Chat interface container with shadow and rounded corners.

**Properties:**
- `background: var(--card-bg)`
- `border-radius: var(--border-radius)`
- `box-shadow: var(--shadow)`
- `overflow: hidden`

### Message Styling

#### `.message`
Base message container with animation.

**Properties:**
- `display: flex`
- `gap: 12px`
- `max-width: 85%`
- `animation: fadeIn 0.3s ease-out`

#### `.message-user`
User message alignment and styling.

**Properties:**
- `align-self: flex-end`
- `flex-direction: row-reverse`

#### `.message-content-user`
User message bubble styling.

**Properties:**
- `background: var(--user-bubble)`
- `color: white`
- `border-bottom-right-radius: 0.25rem`

#### `.message-content-ai`
AI message bubble styling.

**Properties:**
- `background: var(--ai-bubble)`
- `color: var(--text-dark)`
- `border-bottom-left-radius: 0.25rem`

### Interactive Elements

#### `.chat-input`
Main text input field with focus states.

**Properties:**
- `border-radius: 2rem`
- `padding: 0.9rem 1.25rem`
- `transition: all 0.3s`

**Focus State:**
- `border-color: var(--primary)`
- `box-shadow: 0 0 0 3px rgba(110, 72, 170, 0.1)`

#### `.send-button`
Circular send button with hover effects.

**Properties:**
- `width: 50px`
- `height: 50px`
- `background: var(--gradient)`
- `border-radius: 50%`

**Hover State:**
- `transform: translateY(-2px)`
- `box-shadow: 0 5px 15px rgba(110, 72, 170, 0.3)`

#### `.suggestion-chip`
Clickable suggestion buttons.

**Properties:**
- `padding: 0.5rem 1rem`
- `background: rgba(110, 72, 170, 0.1)`
- `border-radius: 2rem`
- `cursor: pointer`

### Animations

#### `fadeIn`
Message appearance animation.

```css
@keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
}
```

#### `typingAnimation`
Typing indicator dot animation.

```css
@keyframes typingAnimation {
    0%, 60%, 100% { transform: translateY(0); }
    30% { transform: translateY(-4px); }
}
```

## HTML Structure

### Main Application Structure
```html
<div class="app-container">
    <header>
        <div class="logo">
            <div class="logo-icon">
                <i class="fas fa-robot"></i>
            </div>
            <div class="logo-text">NexusAI</div>
        </div>
        <button id="new-chat">New Chat</button>
    </header>
    
    <div class="chat-container">
        <div class="chat-header">...</div>
        <div class="chat-messages" id="chat-messages">...</div>
        <div class="chat-input-container">...</div>
    </div>
</div>
```

### Form Structure
```html
<form class="chat-form" id="chat-form">
    <textarea 
        class="chat-input" 
        id="chat-input" 
        placeholder="Message NexusAI..." 
        rows="1"
        required
    ></textarea>
    <button type="submit" class="send-button">
        <i class="fas fa-paper-plane"></i>
    </button>
</form>
```

## Event Handlers

### Form Submission
```javascript
chatForm.addEventListener('submit', function(e) {
    e.preventDefault();
    sendMessage();
});
```

### Input Auto-resize
```javascript
chatInput.addEventListener('input', function() {
    this.style.height = 'auto';
    this.style.height = (this.scrollHeight) + 'px';
});
```

### New Chat Button
```javascript
newChatButton.addEventListener('click', function() {
    if (confirm('Start a new chat? Current conversation will be cleared.')) {
        // Clear messages and reset chat
    }
});
```

### Suggestion Chips
```javascript
suggestionChips.forEach(chip => {
    chip.addEventListener('click', function() {
        chatInput.value = this.textContent;
        chatInput.focus();
    });
});
```

## Local Storage API

### Save Chat History
```javascript
function saveChatHistory() {
    const messages = [];
    document.querySelectorAll('.message').forEach(el => {
        const sender = el.classList.contains('message-ai') ? 'ai' : 'user';
        const content = el.querySelector('.message-content').textContent;
        messages.push({ sender, content });
    });
    localStorage.setItem('chatHistory', JSON.stringify(messages));
}
```

### Load Chat History
```javascript
function loadChatHistory() {
    const history = localStorage.getItem('chatHistory');
    if (history) {
        const messages = JSON.parse(history);
        messages.forEach(msg => {
            if (!msg.content.includes("Hello! I'm NexusAI")) {
                addMessage(msg.content, msg.sender);
            }
        });
    }
}
```

## External API Integration

### Current Implementation
The application includes a placeholder for external API integration:

```javascript
async function generateAIResponse(userMessage) {
    try {
        const response = await fetch('YOUR_API_ENDPOINT', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ message: userMessage })
        });
        const data = await response.json();
        return data.reply;
    } catch (error) {
        console.error("API Error:", error);
        return "Sorry, I'm having trouble responding. Please try again.";
    }
}
```

### Integration Steps
1. Replace `'YOUR_API_ENDPOINT'` with actual API URL
2. Modify request body format as needed
3. Update response parsing logic
4. Add authentication headers if required

### Example API Integrations

#### OpenAI API
```javascript
async function generateAIResponse(userMessage) {
    try {
        const response = await fetch('https://api.openai.com/v1/chat/completions', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer YOUR_API_KEY'
            },
            body: JSON.stringify({
                model: "gpt-3.5-turbo",
                messages: [{ role: "user", content: userMessage }]
            })
        });
        const data = await response.json();
        return data.choices[0].message.content;
    } catch (error) {
        console.error("API Error:", error);
        return "Sorry, I'm having trouble responding. Please try again.";
    }
}
```

## Usage Examples

### Basic Chat Interaction
```javascript
// Send a message programmatically
addMessage("Hello, how are you?", "user");

// Generate and display AI response
const response = await generateAIResponse("Hello, how are you?");
addMessage(response, "ai");

// Save the conversation
saveChatHistory();
```

### Custom Message Handling
```javascript
// Add message with custom styling
function addCustomMessage(content, sender, className) {
    const messageDiv = document.createElement('div');
    messageDiv.className = `message message-${sender} ${className}`;
    
    const avatarDiv = document.createElement('div');
    avatarDiv.className = `avatar avatar-${sender}`;
    avatarDiv.innerHTML = sender === 'ai' ? '<i class="fas fa-robot"></i>' : '<i class="fas fa-user"></i>';
    
    const contentDiv = document.createElement('div');
    contentDiv.className = `message-content message-content-${sender}`;
    contentDiv.innerHTML = content; // Use innerHTML for rich content
    
    messageDiv.appendChild(avatarDiv);
    messageDiv.appendChild(contentDiv);
    
    document.getElementById('chat-messages').appendChild(messageDiv);
    scrollToBottom();
}
```

### Suggestion Management
```javascript
// Add new suggestion chips
function addSuggestion(text) {
    const suggestionsContainer = document.querySelector('.suggestions');
    const chip = document.createElement('div');
    chip.className = 'suggestion-chip';
    chip.textContent = text;
    chip.addEventListener('click', function() {
        document.getElementById('chat-input').value = this.textContent;
        document.getElementById('chat-input').focus();
    });
    suggestionsContainer.appendChild(chip);
}

// Remove all suggestions
function clearSuggestions() {
    const suggestionsContainer = document.querySelector('.suggestions');
    suggestionsContainer.innerHTML = '';
}
```

### Chat Management
```javascript
// Clear chat history
function clearChat() {
    const chatMessages = document.getElementById('chat-messages');
    while (chatMessages.children.length > 2) {
        chatMessages.removeChild(chatMessages.lastChild);
    }
    localStorage.removeItem('chatHistory');
}

// Export chat history
function exportChatHistory() {
    const history = localStorage.getItem('chatHistory');
    if (history) {
        const blob = new Blob([history], { type: 'application/json' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'chat-history.json';
        a.click();
        URL.revokeObjectURL(url);
    }
}
```

## Configuration

### Theme Customization
Modify CSS variables in `:root` to change the theme:

```css
:root {
    --primary: #your-primary-color;
    --primary-light: #your-light-color;
    --bg-color: #your-background-color;
    --card-bg: #your-card-background;
    --text-dark: #your-text-color;
    --border-radius: 16px; /* Adjust for different roundness */
}
```

### Responsive Breakpoints
```css
/* Mobile */
@media (max-width: 768px) {
    .app-container { padding: 1rem; }
    .message { max-width: 90%; }
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
    :root {
        --bg-color: #1a1a2e;
        --card-bg: #16213e;
        --text-dark: #ffffff;
    }
}
```

### Default Suggestions
Modify the suggestions array in HTML:

```html
<div class="suggestions">
    <div class="suggestion-chip">Your custom suggestion</div>
    <div class="suggestion-chip">Another suggestion</div>
    <!-- Add more as needed -->
</div>
```

### API Configuration
Update the API endpoint and request format:

```javascript
const API_CONFIG = {
    endpoint: 'https://your-api-endpoint.com',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer YOUR_TOKEN'
    },
    timeout: 10000
};
```

## Browser Compatibility

- **Modern Browsers**: Chrome 60+, Firefox 55+, Safari 12+, Edge 79+
- **Features Used**: 
  - CSS Grid and Flexbox
  - CSS Custom Properties (Variables)
  - Async/Await
  - LocalStorage API
  - Fetch API

## Performance Considerations

- Messages are stored in DOM (consider virtualization for large chat histories)
- LocalStorage has size limits (typically 5-10MB)
- API calls include error handling and timeouts
- CSS animations use transform for better performance

## Security Notes

- Input is not sanitized (add XSS protection for production)
- API keys should be handled server-side
- Consider rate limiting for API calls
- Validate all user inputs before processing

---

*This documentation covers all public APIs, functions, and components of the NexusAI chatbot application. For additional customization or integration questions, refer to the source code in `chat-bot.html`.*