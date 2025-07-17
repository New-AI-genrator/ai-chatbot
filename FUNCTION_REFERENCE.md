# NexusAI - Function Reference

## Overview

This document provides a complete reference for all JavaScript functions in the NexusAI chatbot application. Each function is documented with its purpose, parameters, return values, and usage examples.

## Table of Contents

1. [Core Functions](#core-functions)
2. [Message Management](#message-management)
3. [Storage Functions](#storage-functions)
4. [UI Utility Functions](#ui-utility-functions)
5. [Event Handlers](#event-handlers)
6. [API Functions](#api-functions)
7. [Helper Functions](#helper-functions)

## Core Functions

### `sendMessage()`

**Purpose**: Orchestrates the complete message sending flow from user input to AI response.

**Signature**:
```javascript
function sendMessage()
```

**Parameters**: None (reads from global input element)

**Returns**: `void`

**Functionality**:
1. Validates input is not empty
2. Adds user message to chat
3. Clears and resets input field
4. Shows typing indicator
5. Triggers AI response generation
6. Hides typing indicator
7. Adds AI response to chat
8. Saves conversation history

**Usage Example**:
```javascript
// Called automatically on form submission
chatForm.addEventListener('submit', function(e) {
    e.preventDefault();
    sendMessage();
});

// Can also be called programmatically
sendMessage();
```

**Dependencies**:
- `chatInput` (global element)
- `addMessage()`
- `generateAIResponse()`
- `saveChatHistory()`

**Error Handling**:
- Silently ignores empty messages
- Handles API errors through `generateAIResponse()`

---

### `addMessage(content, sender)`

**Purpose**: Creates and displays a new message in the chat interface.

**Signature**:
```javascript
function addMessage(content, sender)
```

**Parameters**:
- `content` (string): The message text to display
- `sender` (string): Message sender type - either `'user'` or `'ai'`

**Returns**: `void`

**Functionality**:
1. Creates message DOM structure
2. Applies appropriate styling based on sender
3. Inserts message into chat container
4. Triggers scroll to bottom
5. Applies fade-in animation

**Usage Examples**:
```javascript
// Add user message
addMessage("Hello, how are you?", "user");

// Add AI response
addMessage("I'm doing well, thank you for asking!", "ai");

// Add system message (custom implementation)
addMessage("Connection established", "system");
```

**DOM Structure Created**:
```html
<div class="message message-user|message-ai">
    <div class="avatar avatar-user|avatar-ai">
        <i class="fas fa-user|fa-robot"></i>
    </div>
    <div class="message-content message-content-user|message-content-ai">
        [content]
    </div>
</div>
```

**Side Effects**:
- Modifies DOM by adding message element
- Triggers `scrollToBottom()`
- Applies CSS animations

---

### `generateAIResponse(userMessage)`

**Purpose**: Generates or fetches AI response to user input.

**Signature**:
```javascript
async function generateAIResponse(userMessage)
```

**Parameters**:
- `userMessage` (string): The user's input message

**Returns**: `Promise<string>` - AI response text

**Current Implementation**:
- Returns randomized response from predefined array
- Includes user message context in response
- Simulates API call structure

**Usage Examples**:
```javascript
// Basic usage
const response = await generateAIResponse("What is AI?");
console.log(response); // "I understand you're asking about 'What is AI?'..."

// With error handling
try {
    const response = await generateAIResponse("Hello");
    addMessage(response, "ai");
} catch (error) {
    console.error("Failed to generate response:", error);
    addMessage("Sorry, I'm having trouble responding.", "ai");
}
```

**Predefined Responses**:
```javascript
const responses = [
    `I understand you're asking about "${userMessage}". Here's what I know...`,
    `Regarding "${userMessage}", there are several important points...`,
    `"${userMessage}" is interesting. Based on my knowledge...`,
    `Let me analyze your question about "${userMessage}"...`,
    `I've researched "${userMessage}" and found these insights...`
];
```

**API Integration Template**:
```javascript
async function generateAIResponse(userMessage) {
    try {
        const response = await fetch('YOUR_API_ENDPOINT', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'Authorization': 'Bearer YOUR_API_KEY'
            },
            body: JSON.stringify({
                message: userMessage,
                // Additional parameters...
            })
        });
        
        if (!response.ok) {
            throw new Error(`API Error: ${response.status}`);
        }
        
        const data = await response.json();
        return data.reply || data.message || data.response;
    } catch (error) {
        console.error("API Error:", error);
        return "Sorry, I'm having trouble responding. Please try again.";
    }
}
```

---

## Message Management

### `scrollToBottom()`

**Purpose**: Scrolls the chat messages container to show the latest message.

**Signature**:
```javascript
function scrollToBottom()
```

**Parameters**: None

**Returns**: `void`

**Implementation**:
```javascript
function scrollToBottom() {
    chatMessages.scrollTop = chatMessages.scrollHeight;
}
```

**Usage Examples**:
```javascript
// Called automatically after adding messages
addMessage("Hello", "user");
// scrollToBottom() is called internally

// Can be called manually
scrollToBottom();

// Smooth scrolling alternative
function smoothScrollToBottom() {
    chatMessages.scrollTo({
        top: chatMessages.scrollHeight,
        behavior: 'smooth'
    });
}
```

**Dependencies**:
- `chatMessages` (global element reference)

---

## Storage Functions

### `saveChatHistory()`

**Purpose**: Persists current chat conversation to localStorage.

**Signature**:
```javascript
function saveChatHistory()
```

**Parameters**: None

**Returns**: `void`

**Functionality**:
1. Extracts all messages from DOM
2. Converts to JSON format
3. Stores in localStorage under 'chatHistory' key

**Storage Format**:
```javascript
[
    {
        "sender": "ai",
        "content": "Hello! I'm NexusAI, your intelligent assistant."
    },
    {
        "sender": "user", 
        "content": "Hi there!"
    },
    {
        "sender": "ai",
        "content": "How can I help you today?"
    }
]
```

**Usage Examples**:
```javascript
// Save current conversation
saveChatHistory();

// Save after adding messages
addMessage("New message", "user");
saveChatHistory();

// Check storage size
const history = localStorage.getItem('chatHistory');
console.log(`Storage size: ${history.length} characters`);
```

**Error Handling**:
```javascript
function saveChatHistory() {
    try {
        const messages = [];
        document.querySelectorAll('.message').forEach(el => {
            const sender = el.classList.contains('message-ai') ? 'ai' : 'user';
            const content = el.querySelector('.message-content').textContent;
            messages.push({ sender, content });
        });
        localStorage.setItem('chatHistory', JSON.stringify(messages));
    } catch (error) {
        console.error('Failed to save chat history:', error);
        // Handle quota exceeded or other storage errors
    }
}
```

---

### `loadChatHistory()`

**Purpose**: Loads and displays previously saved chat history from localStorage.

**Signature**:
```javascript
function loadChatHistory()
```

**Parameters**: None

**Returns**: `void`

**Functionality**:
1. Retrieves stored history from localStorage
2. Parses JSON data
3. Recreates messages in chat interface
4. Filters out default welcome message

**Usage Examples**:
```javascript
// Load on page initialization
document.addEventListener('DOMContentLoaded', function() {
    loadChatHistory();
});

// Reload history manually
loadChatHistory();

// Clear and reload
localStorage.removeItem('chatHistory');
loadChatHistory(); // Will show empty chat
```

**Implementation Details**:
```javascript
function loadChatHistory() {
    const history = localStorage.getItem('chatHistory');
    if (history) {
        try {
            const messages = JSON.parse(history);
            messages.forEach(msg => {
                // Skip default welcome message to avoid duplicates
                if (!msg.content.includes("Hello! I'm NexusAI")) {
                    addMessage(msg.content, msg.sender);
                }
            });
        } catch (error) {
            console.error('Failed to load chat history:', error);
            localStorage.removeItem('chatHistory'); // Clear corrupted data
        }
    }
}
```

---

## UI Utility Functions

### Input Auto-resize Handler

**Purpose**: Automatically adjusts textarea height based on content.

**Signature**:
```javascript
function handleInputResize(event)
```

**Parameters**:
- `event` (Event): Input event from textarea

**Returns**: `void`

**Implementation**:
```javascript
chatInput.addEventListener('input', function() {
    this.style.height = 'auto';
    this.style.height = (this.scrollHeight) + 'px';
});
```

**Usage Examples**:
```javascript
// Applied automatically to chat input
// No manual calling required

// Can be applied to other textareas
function makeAutoResize(textarea) {
    textarea.addEventListener('input', function() {
        this.style.height = 'auto';
        this.style.height = (this.scrollHeight) + 'px';
    });
}

// Usage
const customTextarea = document.getElementById('custom-input');
makeAutoResize(customTextarea);
```

**Constraints**:
- Minimum height: 50px (CSS)
- Maximum height: 150px (CSS)
- Smooth transitions enabled

---

### Typing Indicator Control

**Purpose**: Shows/hides the typing indicator animation.

**Functions**:
```javascript
function showTypingIndicator() {
    document.getElementById('typing-indicator').style.display = 'block';
    scrollToBottom();
}

function hideTypingIndicator() {
    document.getElementById('typing-indicator').style.display = 'none';
}
```

**Usage Examples**:
```javascript
// Show before API call
showTypingIndicator();

// Hide after response
setTimeout(() => {
    hideTypingIndicator();
    addMessage("AI response here", "ai");
}, 2000);

// Toggle based on condition
function toggleTypingIndicator(show) {
    const indicator = document.getElementById('typing-indicator');
    indicator.style.display = show ? 'block' : 'none';
    if (show) scrollToBottom();
}
```

---

## Event Handlers

### Form Submission Handler

**Purpose**: Handles chat form submission and prevents default behavior.

**Signature**:
```javascript
function handleFormSubmit(event)
```

**Parameters**:
- `event` (Event): Form submission event

**Returns**: `void`

**Implementation**:
```javascript
chatForm.addEventListener('submit', function(e) {
    e.preventDefault();
    sendMessage();
});
```

**Alternative Implementations**:
```javascript
// With validation
chatForm.addEventListener('submit', function(e) {
    e.preventDefault();
    
    const message = chatInput.value.trim();
    if (message.length === 0) {
        chatInput.focus();
        return;
    }
    
    if (message.length > 1000) {
        alert('Message too long. Please keep it under 1000 characters.');
        return;
    }
    
    sendMessage();
});

// With rate limiting
let lastMessageTime = 0;
const MESSAGE_COOLDOWN = 1000; // 1 second

chatForm.addEventListener('submit', function(e) {
    e.preventDefault();
    
    const now = Date.now();
    if (now - lastMessageTime < MESSAGE_COOLDOWN) {
        return; // Too soon, ignore
    }
    
    lastMessageTime = now;
    sendMessage();
});
```

---

### New Chat Handler

**Purpose**: Clears current conversation and starts fresh.

**Signature**:
```javascript
function handleNewChat(event)
```

**Parameters**:
- `event` (Event): Click event from new chat button

**Returns**: `void`

**Implementation**:
```javascript
newChatButton.addEventListener('click', function() {
    if (confirm('Start a new chat? Current conversation will be cleared.')) {
        clearChat();
        addMessage("Hello! What would you like to talk about?", 'ai');
    }
});
```

**Helper Function**:
```javascript
function clearChat() {
    const chatMessages = document.getElementById('chat-messages');
    
    // Remove all messages except typing indicator
    while (chatMessages.children.length > 2) {
        chatMessages.removeChild(chatMessages.lastChild);
    }
    
    // Clear stored history
    localStorage.removeItem('chatHistory');
}
```

**Alternative Implementations**:
```javascript
// Without confirmation
function handleNewChatDirect() {
    clearChat();
    addMessage("Hello! How can I help you today?", 'ai');
}

// With animation
function handleNewChatAnimated() {
    const messages = document.querySelectorAll('.message');
    messages.forEach((msg, index) => {
        setTimeout(() => {
            msg.style.opacity = '0';
            msg.style.transform = 'translateY(-20px)';
            setTimeout(() => msg.remove(), 300);
        }, index * 100);
    });
    
    setTimeout(() => {
        localStorage.removeItem('chatHistory');
        addMessage("Hello! Ready for a new conversation?", 'ai');
    }, messages.length * 100 + 300);
}
```

---

### Suggestion Chip Handler

**Purpose**: Handles clicks on suggestion chips to populate input.

**Signature**:
```javascript
function handleSuggestionClick(event)
```

**Parameters**:
- `event` (Event): Click event from suggestion chip

**Returns**: `void`

**Implementation**:
```javascript
suggestionChips.forEach(chip => {
    chip.addEventListener('click', function() {
        chatInput.value = this.textContent;
        chatInput.focus();
    });
});
```

**Enhanced Implementation**:
```javascript
function handleSuggestionClick(event) {
    const suggestion = event.target.textContent;
    
    // Populate input
    chatInput.value = suggestion;
    
    // Focus and position cursor at end
    chatInput.focus();
    chatInput.setSelectionRange(suggestion.length, suggestion.length);
    
    // Auto-resize if needed
    chatInput.style.height = 'auto';
    chatInput.style.height = (chatInput.scrollHeight) + 'px';
    
    // Optional: Auto-send after short delay
    setTimeout(() => {
        if (chatInput.value === suggestion) {
            sendMessage();
        }
    }, 500);
}
```

---

## API Functions

### Fetch with Timeout

**Purpose**: Makes API calls with timeout handling.

**Signature**:
```javascript
async function fetchWithTimeout(url, options, timeout = 10000)
```

**Parameters**:
- `url` (string): API endpoint URL
- `options` (object): Fetch options (method, headers, body, etc.)
- `timeout` (number): Timeout in milliseconds (default: 10000)

**Returns**: `Promise<Response>`

**Implementation**:
```javascript
async function fetchWithTimeout(url, options, timeout = 10000) {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), timeout);
    
    try {
        const response = await fetch(url, {
            ...options,
            signal: controller.signal
        });
        clearTimeout(timeoutId);
        return response;
    } catch (error) {
        clearTimeout(timeoutId);
        if (error.name === 'AbortError') {
            throw new Error('Request timeout');
        }
        throw error;
    }
}
```

**Usage Examples**:
```javascript
// Basic usage
try {
    const response = await fetchWithTimeout('/api/chat', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ message: 'Hello' })
    });
    const data = await response.json();
} catch (error) {
    console.error('API call failed:', error);
}

// With custom timeout
const response = await fetchWithTimeout('/api/chat', options, 5000);
```

---

### API Error Handler

**Purpose**: Standardizes API error handling and user feedback.

**Signature**:
```javascript
function handleAPIError(error, userMessage = "Sorry, I'm having trouble responding.")
```

**Parameters**:
- `error` (Error): The error object
- `userMessage` (string): User-friendly error message

**Returns**: `string` - Error message to display

**Implementation**:
```javascript
function handleAPIError(error, userMessage = "Sorry, I'm having trouble responding.") {
    console.error('API Error:', error);
    
    // Log detailed error information
    const errorInfo = {
        message: error.message,
        stack: error.stack,
        timestamp: new Date().toISOString(),
        userAgent: navigator.userAgent
    };
    
    // Send to error tracking service (optional)
    // trackError(errorInfo);
    
    // Return user-friendly message
    if (error.message.includes('timeout')) {
        return "I'm taking longer than usual to respond. Please try again.";
    } else if (error.message.includes('network')) {
        return "Connection problem. Please check your internet and try again.";
    } else {
        return userMessage;
    }
}
```

**Usage Examples**:
```javascript
// In generateAIResponse
try {
    const response = await fetchWithTimeout(apiUrl, options);
    return await response.json();
} catch (error) {
    return handleAPIError(error);
}

// With custom message
try {
    // API call
} catch (error) {
    const errorMsg = handleAPIError(error, "Unable to process your request right now.");
    addMessage(errorMsg, "ai");
}
```

---

## Helper Functions

### Message Validation

**Purpose**: Validates user input before processing.

**Signature**:
```javascript
function validateMessage(message)
```

**Parameters**:
- `message` (string): User input to validate

**Returns**: `object` - Validation result with `isValid` and `error` properties

**Implementation**:
```javascript
function validateMessage(message) {
    const result = { isValid: true, error: null };
    
    // Check if empty
    if (!message || message.trim().length === 0) {
        result.isValid = false;
        result.error = "Message cannot be empty";
        return result;
    }
    
    // Check length
    if (message.length > 1000) {
        result.isValid = false;
        result.error = "Message too long (max 1000 characters)";
        return result;
    }
    
    // Check for spam patterns
    const spamPatterns = [
        /(.)\1{10,}/, // Repeated characters
        /^[A-Z\s!]{20,}$/, // All caps
    ];
    
    for (const pattern of spamPatterns) {
        if (pattern.test(message)) {
            result.isValid = false;
            result.error = "Message appears to be spam";
            return result;
        }
    }
    
    return result;
}
```

**Usage Examples**:
```javascript
// Before sending message
function sendMessage() {
    const message = chatInput.value.trim();
    const validation = validateMessage(message);
    
    if (!validation.isValid) {
        alert(validation.error);
        return;
    }
    
    // Continue with message sending...
}
```

---

### Text Sanitization

**Purpose**: Sanitizes user input to prevent XSS attacks.

**Signature**:
```javascript
function sanitizeText(text)
```

**Parameters**:
- `text` (string): Text to sanitize

**Returns**: `string` - Sanitized text

**Implementation**:
```javascript
function sanitizeText(text) {
    const div = document.createElement('div');
    div.textContent = text;
    return div.innerHTML;
}

// More comprehensive sanitization
function sanitizeHTML(html) {
    const allowedTags = ['b', 'i', 'em', 'strong', 'br'];
    const div = document.createElement('div');
    div.innerHTML = html;
    
    // Remove script tags and event handlers
    const scripts = div.querySelectorAll('script');
    scripts.forEach(script => script.remove());
    
    // Remove dangerous attributes
    const allElements = div.querySelectorAll('*');
    allElements.forEach(el => {
        // Remove event handlers
        for (let i = el.attributes.length - 1; i >= 0; i--) {
            const attr = el.attributes[i];
            if (attr.name.startsWith('on')) {
                el.removeAttribute(attr.name);
            }
        }
        
        // Remove non-allowed tags
        if (!allowedTags.includes(el.tagName.toLowerCase())) {
            el.outerHTML = el.innerHTML;
        }
    });
    
    return div.innerHTML;
}
```

---

### Format Timestamp

**Purpose**: Formats timestamps for message display.

**Signature**:
```javascript
function formatTimestamp(date = new Date())
```

**Parameters**:
- `date` (Date): Date object to format (default: current date)

**Returns**: `string` - Formatted timestamp

**Implementation**:
```javascript
function formatTimestamp(date = new Date()) {
    const now = new Date();
    const diff = now - date;
    
    // Less than 1 minute
    if (diff < 60000) {
        return 'Just now';
    }
    
    // Less than 1 hour
    if (diff < 3600000) {
        const minutes = Math.floor(diff / 60000);
        return `${minutes} minute${minutes > 1 ? 's' : ''} ago`;
    }
    
    // Less than 1 day
    if (diff < 86400000) {
        const hours = Math.floor(diff / 3600000);
        return `${hours} hour${hours > 1 ? 's' : ''} ago`;
    }
    
    // Format as date
    return date.toLocaleDateString();
}
```

**Usage Examples**:
```javascript
// Add timestamp to messages
function addMessageWithTimestamp(content, sender) {
    const timestamp = formatTimestamp();
    const messageWithTime = `${content} <span class="timestamp">${timestamp}</span>`;
    addMessage(messageWithTime, sender);
}

// Display in message hover
function addTimestampTooltip(messageElement) {
    const timestamp = formatTimestamp(new Date(messageElement.dataset.timestamp));
    messageElement.title = `Sent ${timestamp}`;
}
```

---

### Export Chat History

**Purpose**: Exports chat history in various formats.

**Signature**:
```javascript
function exportChatHistory(format = 'json')
```

**Parameters**:
- `format` (string): Export format - 'json', 'txt', or 'html'

**Returns**: `void` (triggers download)

**Implementation**:
```javascript
function exportChatHistory(format = 'json') {
    const history = localStorage.getItem('chatHistory');
    if (!history) {
        alert('No chat history to export');
        return;
    }
    
    const messages = JSON.parse(history);
    let content, mimeType, filename;
    
    switch (format) {
        case 'json':
            content = JSON.stringify(messages, null, 2);
            mimeType = 'application/json';
            filename = 'chat-history.json';
            break;
            
        case 'txt':
            content = messages.map(msg => 
                `${msg.sender.toUpperCase()}: ${msg.content}`
            ).join('\n\n');
            mimeType = 'text/plain';
            filename = 'chat-history.txt';
            break;
            
        case 'html':
            content = generateHTMLExport(messages);
            mimeType = 'text/html';
            filename = 'chat-history.html';
            break;
            
        default:
            throw new Error('Unsupported format');
    }
    
    const blob = new Blob([content], { type: mimeType });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    a.click();
    URL.revokeObjectURL(url);
}

function generateHTMLExport(messages) {
    const html = `
<!DOCTYPE html>
<html>
<head>
    <title>NexusAI Chat History</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .message { margin: 10px 0; padding: 10px; border-radius: 5px; }
        .user { background: #e3f2fd; text-align: right; }
        .ai { background: #f5f5f5; }
        .sender { font-weight: bold; }
    </style>
</head>
<body>
    <h1>NexusAI Chat History</h1>
    <div class="chat">
        ${messages.map(msg => `
            <div class="message ${msg.sender}">
                <div class="sender">${msg.sender.toUpperCase()}</div>
                <div class="content">${msg.content}</div>
            </div>
        `).join('')}
    </div>
</body>
</html>`;
    return html;
}
```

---

*This function reference provides complete documentation for all JavaScript functions in the NexusAI chatbot application. Use this as a reference for development, debugging, and maintenance.*