# NexusAI - Component Documentation

## Overview

This document provides detailed information about the reusable components in the NexusAI chatbot application. Each component is designed to be modular and easily customizable.

## Component Architecture

```
NexusAI Application
├── AppContainer
│   ├── Header
│   │   ├── Logo
│   │   └── NewChatButton
│   └── ChatContainer
│       ├── ChatHeader
│       ├── ChatMessages
│       │   ├── Message (User/AI)
│       │   │   ├── Avatar
│       │   │   └── MessageContent
│       │   └── TypingIndicator
│       └── ChatInputContainer
│           ├── Suggestions
│           │   └── SuggestionChip
│           └── ChatForm
│               ├── ChatInput
│               └── SendButton
```

## Core Components

### 1. AppContainer

**Purpose**: Main application wrapper providing layout and responsive behavior.

**HTML Structure**:
```html
<div class="app-container">
  <!-- Application content -->
</div>
```

**CSS Properties**:
- `max-width: 1200px` - Maximum container width
- `margin: 0 auto` - Center alignment
- `padding: 2rem` - Internal spacing
- `flex: 1` - Flexible layout

**Responsive Behavior**:
- Mobile: `padding: 1rem`
- Maintains center alignment across all screen sizes

**Usage Example**:
```javascript
// The app container is automatically applied to the root element
// No direct JavaScript interaction required
```

### 2. Header

**Purpose**: Application header with branding and navigation.

**HTML Structure**:
```html
<header>
  <div class="logo">
    <div class="logo-icon">
      <i class="fas fa-robot"></i>
    </div>
    <div class="logo-text">NexusAI</div>
  </div>
  <button id="new-chat">
    <i class="fas fa-plus"></i> New Chat
  </button>
</header>
```

**Components**:
- **Logo**: Brand identifier with icon and text
- **NewChatButton**: Action button for starting new conversations

**Styling**:
- Flexbox layout with space-between alignment
- Gradient text effect on logo
- Hover effects on button

### 3. Logo

**Purpose**: Brand representation with icon and text.

**Properties**:
- `logo-icon`: 36x36px container with gradient background
- `logo-text`: Gradient text effect using background-clip

**Customization**:
```css
.logo-icon {
  width: 36px;
  height: 36px;
  background: var(--gradient);
  border-radius: 10px;
}

.logo-text {
  font-size: 1.5rem;
  font-weight: 700;
  background: var(--gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
```

### 4. ChatContainer

**Purpose**: Main chat interface wrapper.

**HTML Structure**:
```html
<div class="chat-container">
  <div class="chat-header">...</div>
  <div class="chat-messages" id="chat-messages">...</div>
  <div class="chat-input-container">...</div>
</div>
```

**Features**:
- Card-style design with shadow
- Rounded corners
- Flex layout for optimal space usage
- Overflow handling for scrollable content

**Styling**:
```css
.chat-container {
  flex: 1;
  display: flex;
  flex-direction: column;
  background: var(--card-bg);
  border-radius: var(--border-radius);
  box-shadow: var(--shadow);
  overflow: hidden;
}
```

### 5. Message

**Purpose**: Individual message display component.

**Interface**:
```javascript
// Message creation
function addMessage(content, sender) {
  // content: string - The message text
  // sender: 'user' | 'ai' - Message sender type
}
```

**HTML Structure**:
```html
<div class="message message-user|message-ai">
  <div class="avatar avatar-user|avatar-ai">
    <i class="fas fa-user|fa-robot"></i>
  </div>
  <div class="message-content message-content-user|message-content-ai">
    Message text content
  </div>
</div>
```

**Variants**:
- **User Message**: Right-aligned, purple background
- **AI Message**: Left-aligned, light background

**Animation**:
- Fade-in animation on message appearance
- Smooth transition effects

**Styling Classes**:
```css
.message {
  display: flex;
  gap: 12px;
  max-width: 85%;
  animation: fadeIn 0.3s ease-out;
}

.message-user {
  align-self: flex-end;
  flex-direction: row-reverse;
}

.message-ai {
  align-self: flex-start;
}
```

### 6. Avatar

**Purpose**: User/AI identification icon.

**Properties**:
- Size: 36x36px
- Circular shape
- Icon-based representation
- Color-coded by sender type

**Variants**:
```css
.avatar-user {
  background: var(--user-bubble);
}

.avatar-ai {
  background: var(--gradient);
}
```

**Icons**:
- User: `fas fa-user`
- AI: `fas fa-robot`

### 7. MessageContent

**Purpose**: Message text container with appropriate styling.

**Features**:
- Rounded corners with sender-specific radius
- Padding for readability
- Shadow for depth
- Responsive text sizing

**Styling**:
```css
.message-content {
  padding: 1rem 1.25rem;
  border-radius: 1rem;
  line-height: 1.5;
  font-size: 0.95rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}
```

**Variants**:
- **User**: White text on purple background
- **AI**: Dark text on light background

### 8. TypingIndicator

**Purpose**: Visual feedback when AI is processing.

**HTML Structure**:
```html
<div class="typing-indicator" id="typing-indicator">
  <div class="typing-dots">
    <div class="typing-dot"></div>
    <div class="typing-dot"></div>
    <div class="typing-dot"></div>
  </div>
</div>
```

**Animation**:
```css
@keyframes typingAnimation {
  0%, 60%, 100% { transform: translateY(0); }
  30% { transform: translateY(-4px); }
}

.typing-dot {
  animation: typingAnimation 1.4s infinite ease-in-out;
}
```

**Control Methods**:
```javascript
// Show typing indicator
document.getElementById('typing-indicator').style.display = 'block';

// Hide typing indicator
document.getElementById('typing-indicator').style.display = 'none';
```

### 9. ChatInput

**Purpose**: Message input field with auto-resize functionality.

**HTML Structure**:
```html
<textarea 
  class="chat-input" 
  id="chat-input" 
  placeholder="Message NexusAI..." 
  rows="1"
  required
></textarea>
```

**Features**:
- Auto-resize based on content
- Rounded design
- Focus states with color changes
- Placeholder text

**Auto-resize Implementation**:
```javascript
chatInput.addEventListener('input', function() {
  this.style.height = 'auto';
  this.style.height = (this.scrollHeight) + 'px';
});
```

**Styling**:
```css
.chat-input {
  flex: 1;
  padding: 0.9rem 1.25rem;
  border: 1px solid rgba(0, 0, 0, 0.1);
  border-radius: 2rem;
  font-size: 0.95rem;
  resize: none;
  min-height: 50px;
  max-height: 150px;
  outline: none;
  transition: all 0.3s;
}

.chat-input:focus {
  border-color: var(--primary);
  box-shadow: 0 0 0 3px rgba(110, 72, 170, 0.1);
}
```

### 10. SendButton

**Purpose**: Submit button for sending messages.

**HTML Structure**:
```html
<button type="submit" class="send-button">
  <i class="fas fa-paper-plane"></i>
</button>
```

**Features**:
- Circular design
- Gradient background
- Hover animations
- Icon-based representation

**Styling**:
```css
.send-button {
  width: 50px;
  height: 50px;
  background: var(--gradient);
  color: white;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.3s;
}

.send-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(110, 72, 170, 0.3);
}
```

**Responsive Behavior**:
```css
@media (max-width: 768px) {
  .send-button {
    width: 100%;
    height: 45px;
    border-radius: 0.5rem;
  }
}
```

### 11. SuggestionChip

**Purpose**: Quick-access buttons for common queries.

**HTML Structure**:
```html
<div class="suggestion-chip">Suggestion text</div>
```

**Features**:
- Pill-shaped design
- Hover effects
- Click-to-populate input
- Flexible layout

**Event Handling**:
```javascript
suggestionChips.forEach(chip => {
  chip.addEventListener('click', function() {
    chatInput.value = this.textContent;
    chatInput.focus();
  });
});
```

**Styling**:
```css
.suggestion-chip {
  padding: 0.5rem 1rem;
  background: rgba(110, 72, 170, 0.1);
  border-radius: 2rem;
  font-size: 0.85rem;
  cursor: pointer;
  transition: all 0.3s;
}

.suggestion-chip:hover {
  background: rgba(110, 72, 170, 0.2);
}
```

## Component Interactions

### Message Flow
1. User types in `ChatInput`
2. Form submission triggers `sendMessage()`
3. `addMessage()` creates user message
4. `TypingIndicator` shows
5. `generateAIResponse()` processes message
6. `addMessage()` creates AI response
7. `saveChatHistory()` persists conversation

### Event Chain
```javascript
// User input → Form submission → Message creation → API call → Response display
chatForm.addEventListener('submit', function(e) {
  e.preventDefault();
  sendMessage(); // Orchestrates the entire flow
});
```

## Customization Guide

### Theme Customization

**Color Scheme**:
```css
:root {
  --primary: #6e48aa;           /* Primary brand color */
  --primary-light: #9d50bb;     /* Lighter brand color */
  --bg-color: #f9f9ff;          /* Background color */
  --card-bg: #ffffff;           /* Card background */
  --text-dark: #2e2e48;         /* Dark text */
  --text-light: #7a7a9d;        /* Light text */
  --user-bubble: var(--primary); /* User message color */
  --ai-bubble: #f1f3ff;         /* AI message color */
}
```

**Typography**:
```css
body {
  font-family: 'Inter', sans-serif;
}

.logo-text {
  font-size: 1.5rem;
  font-weight: 700;
}

.message-content {
  font-size: 0.95rem;
  line-height: 1.5;
}
```

### Component Extensions

**Custom Message Types**:
```javascript
function addSystemMessage(content) {
  const messageDiv = document.createElement('div');
  messageDiv.className = 'message message-system';
  messageDiv.innerHTML = `
    <div class="avatar avatar-system">
      <i class="fas fa-info-circle"></i>
    </div>
    <div class="message-content message-content-system">
      ${content}
    </div>
  `;
  document.getElementById('chat-messages').appendChild(messageDiv);
}
```

**Custom Suggestion Management**:
```javascript
class SuggestionManager {
  constructor(containerSelector) {
    this.container = document.querySelector(containerSelector);
  }
  
  addSuggestion(text, callback) {
    const chip = document.createElement('div');
    chip.className = 'suggestion-chip';
    chip.textContent = text;
    chip.addEventListener('click', callback || this.defaultCallback);
    this.container.appendChild(chip);
  }
  
  clearSuggestions() {
    this.container.innerHTML = '';
  }
  
  defaultCallback() {
    document.getElementById('chat-input').value = this.textContent;
    document.getElementById('chat-input').focus();
  }
}

// Usage
const suggestionManager = new SuggestionManager('.suggestions');
suggestionManager.addSuggestion('Custom suggestion');
```

## Accessibility Features

### Keyboard Navigation
- Tab navigation through interactive elements
- Enter key submission for chat form
- Focus management for input field

### Screen Reader Support
- Semantic HTML structure
- Proper heading hierarchy
- Alt text for icons (via FontAwesome)
- ARIA labels where needed

### Recommended Enhancements
```html
<!-- Add ARIA labels -->
<button id="new-chat" aria-label="Start new conversation">
  <i class="fas fa-plus"></i> New Chat
</button>

<!-- Add role attributes -->
<div class="chat-messages" id="chat-messages" role="log" aria-live="polite">
  <!-- Messages -->
</div>

<!-- Add form labels -->
<label for="chat-input" class="sr-only">Type your message</label>
<textarea 
  class="chat-input" 
  id="chat-input" 
  placeholder="Message NexusAI..."
  aria-label="Type your message"
></textarea>
```

## Performance Optimization

### Message Virtualization
For large chat histories, consider implementing virtual scrolling:

```javascript
class VirtualizedMessages {
  constructor(container, itemHeight = 80) {
    this.container = container;
    this.itemHeight = itemHeight;
    this.messages = [];
    this.visibleStart = 0;
    this.visibleEnd = 0;
  }
  
  addMessage(message) {
    this.messages.push(message);
    this.updateVisibleMessages();
  }
  
  updateVisibleMessages() {
    const containerHeight = this.container.clientHeight;
    const visibleCount = Math.ceil(containerHeight / this.itemHeight);
    const scrollTop = this.container.scrollTop;
    
    this.visibleStart = Math.floor(scrollTop / this.itemHeight);
    this.visibleEnd = Math.min(this.visibleStart + visibleCount, this.messages.length);
    
    this.render();
  }
  
  render() {
    // Render only visible messages
    // Implementation depends on specific requirements
  }
}
```

### Memory Management
```javascript
// Limit stored messages
const MAX_MESSAGES = 100;

function addMessage(content, sender) {
  // Add message logic...
  
  // Remove old messages if limit exceeded
  const messages = document.querySelectorAll('.message');
  if (messages.length > MAX_MESSAGES) {
    messages[0].remove();
  }
}
```

## Testing Considerations

### Unit Testing
```javascript
// Example test structure
describe('Message Component', () => {
  test('should create user message with correct styling', () => {
    addMessage('Test message', 'user');
    const message = document.querySelector('.message-user');
    expect(message).toBeTruthy();
    expect(message.textContent).toContain('Test message');
  });
  
  test('should create AI message with correct styling', () => {
    addMessage('AI response', 'ai');
    const message = document.querySelector('.message-ai');
    expect(message).toBeTruthy();
    expect(message.textContent).toContain('AI response');
  });
});
```

### Integration Testing
```javascript
// Test complete message flow
describe('Chat Flow', () => {
  test('should handle complete user interaction', async () => {
    // Simulate user input
    const input = document.getElementById('chat-input');
    input.value = 'Hello';
    
    // Simulate form submission
    const form = document.getElementById('chat-form');
    form.dispatchEvent(new Event('submit'));
    
    // Verify user message appears
    expect(document.querySelector('.message-user')).toBeTruthy();
    
    // Wait for AI response
    await new Promise(resolve => setTimeout(resolve, 2000));
    
    // Verify AI response appears
    expect(document.querySelector('.message-ai')).toBeTruthy();
  });
});
```

---

*This component documentation provides detailed information about each reusable component in the NexusAI chatbot application. Use this as a reference for customization, extension, and maintenance.*