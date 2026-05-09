let todoList = [
  {
    item: 'Buy Milk',
    dueDate: '2026-02-20',
    completed: false
  },
  {
    item: 'Go to College',
    dueDate: '2026-02-18',
    completed: false
  },
  {
    item: 'Complete Assignment',
    dueDate: '2026-02-19',
    completed: true
  }
];

// Initialize app
document.addEventListener('DOMContentLoaded', function() {
  setupTabNavigation();
  displayItems();
  updateStats();
  initChatbot();
});

// Tab Navigation
function setupTabNavigation() {
  const navBtns = document.querySelectorAll('.nav-btn');
  navBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      // Remove active class from all buttons and sections
      document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
      document.querySelectorAll('.interface').forEach(section => section.classList.remove('active'));
      
      // Add active class to clicked button and corresponding section
      btn.classList.add('active');
      const tabId = btn.getAttribute('data-tab');
      document.getElementById(tabId).classList.add('active');
      
      // Update display based on active tab
      if (tabId === 'today') displayTodayTodos();
      else if (tabId === 'completed') displayCompletedTodos();
      else if (tabId === 'dashboard') displayStats();
      else displayItems();
    });
  });
}

// Add New Todo
function addTodo() {
  let inputElement = document.querySelector('#todo-input');
  let dateElement = document.querySelector('#todo-date');
  let todoItem = inputElement.value.trim();
  let todoDate = dateElement.value;
  
  if (todoItem === '' || todoDate === '') {
    alert('Please fill in all fields!');
    return;
  }
  
  todoList.push({
    item: todoItem,
    dueDate: todoDate,
    completed: false
  });
  
  inputElement.value = '';
  dateElement.value = '';
  
  // Show task added animation
  showTaskAnimation();
  
  displayItems();
  updateStats();
}

// Task Added Animation
function showTaskAnimation() {
  const animation = document.createElement('div');
  animation.innerHTML = '✓ DONE!';
  animation.style.cssText = `
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-size: 48px;
    font-weight: 700;
    color: #28a745;
    text-shadow: 0 0 20px rgba(40, 167, 69, 0.6);
    pointer-events: none;
    z-index: 9999;
    animation: doneAnimation 1.2s ease-out forwards;
  `;
  document.body.appendChild(animation);
  
  setTimeout(() => animation.remove(), 1200);
}

// Add animation keyframes to the document
if (!document.getElementById('task-animation-style')) {
  const style = document.createElement('style');
  style.id = 'task-animation-style';
  style.textContent = `
    @keyframes doneAnimation {
      0% {
        opacity: 1;
        transform: translate(-50%, -50%) scale(0.5) rotate(-10deg);
      }
      50% {
        opacity: 1;
        transform: translate(-50%, -60%) scale(1.2) rotate(5deg);
      }
      100% {
        opacity: 0;
        transform: translate(-50%, -150%) scale(1) rotate(0deg);
      }
    }
  `;
  document.head.appendChild(style);
}

// Display All Todos (Home Interface)
function displayItems() {
  let containerElement = document.querySelector('#home-todos');
  
  if (todoList.length === 0) {
    containerElement.innerHTML = '<div class="empty-state"><p>📭 No tasks yet. Add one to get started!</p></div>';
    return;
  }
  
  let newHtml = '';
  for (let i = 0; i < todoList.length; i++) {
    let {item, dueDate, completed} = todoList[i];
    let completedClass = completed ? 'completed' : '';
    newHtml += `
      <div class="todo-item ${completedClass}">
        <span>${item}</span>
        <span>${dueDate}</span>
        <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 8px;">
          <button class='btn-complete' onclick="toggleComplete(${i});">${completed ? 'Undo' : 'Done'}</button>
          <button class='btn-delete' onclick="deleteTodo(${i});">Delete</button>
        </div>
      </div>
    `;
  }
  containerElement.innerHTML = newHtml;
}

// Display Today's Todos
function displayTodayTodos() {
  let containerElement = document.querySelector('#today-todos');
  const today = new Date().toISOString().split('T')[0];
  const todayTodos = todoList.filter(todo => todo.dueDate === today && !todo.completed);
  
  if (todayTodos.length === 0) {
    containerElement.innerHTML = '<div class="empty-state"><p>📋 No tasks for today!</p></div>';
    return;
  }
  
  let newHtml = '';
  for (let i = 0; i < todoList.length; i++) {
    let {item, dueDate, completed} = todoList[i];
    if (dueDate === today && !completed) {
      newHtml += `
        <div class="todo-item">
          <span>${item}</span>
          <span>${dueDate}</span>
          <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 8px;">
            <button class='btn-complete' onclick="toggleComplete(${i});">Done</button>
            <button class='btn-delete' onclick="deleteTodo(${i});">Delete</button>
          </div>
        </div>
      `;
    }
  }
  containerElement.innerHTML = newHtml;
}

// Display Completed Todos
function displayCompletedTodos() {
  let containerElement = document.querySelector('#completed-todos');
  const completedTodos = todoList.filter(todo => todo.completed);
  
  if (completedTodos.length === 0) {
    containerElement.innerHTML = '<div class="empty-state"><p>✅ Complete some tasks to see them here!</p></div>';
    return;
  }
  
  let newHtml = '';
  for (let i = 0; i < todoList.length; i++) {
    let {item, dueDate, completed} = todoList[i];
    if (completed) {
      newHtml += `
        <div class="todo-item completed">
          <span>✓ ${item}</span>
          <span>${dueDate}</span>
          <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 8px;">
            <button class='btn-complete' onclick="toggleComplete(${i});">Undo</button>
            <button class='btn-delete' onclick="deleteTodo(${i});">Delete</button>
          </div>
        </div>
      `;
    }
  }
  containerElement.innerHTML = newHtml;
}

// Toggle Complete Status
function toggleComplete(index) {
  todoList[index].completed = !todoList[index].completed;
  displayItems();
  updateStats();
}

// Delete Todo
function deleteTodo(index) {
  if (confirm('Are you sure you want to delete this task?')) {
    todoList.splice(index, 1);
    displayItems();
    updateStats();
  }
}

// Display Statistics
function displayStats() {
  updateStats();
}

// Update Statistics
function updateStats() {
  const total = todoList.length;
  const completed = todoList.filter(todo => todo.completed).length;
  const pending = total - completed;
  const rate = total > 0 ? Math.round((completed / total) * 100) : 0;
  
  document.getElementById('total-tasks').textContent = total;
  document.getElementById('completed-tasks').textContent = completed;
  document.getElementById('pending-tasks').textContent = pending;
  document.getElementById('completion-rate').textContent = rate + '%';
}

// Chatbot Functions
const chatbotResponses = {
  hello: "Hi there! 👋 Welcome to your TODO app! How can I help you today?",
  help: "I can help you with:\\n• Adding new tasks\\n• Marking tasks complete\\n• Viewing today's tasks\\n• Checking your progress\\nJust ask! 😊",
  tasks: `You have ${todoList.length} total tasks. ${todoList.filter(t => t.completed).length} are completed! Keep going! 🚀`,
  today: `You have ${todoList.filter(t => !t.completed && new Date(t.dueDate).toDateString() === new Date().toDateString()).length} tasks for today! 📅`,
  add: "To add a task, go to the Home section and type your task in the input field, select a date, and click Add! 📝",
  complete: "To mark a task complete, click the 'Done' button next to any task! ✅",
  dashboard: "The Dashboard shows your progress with statistics! Check it out in the sidebar! 📊",
  motivation: "You're doing great! Keep completing those tasks and stay productive! 💪✨",
  default: "I'm not sure about that, but you're doing amazing with your todos! 😊"
};

function initChatbot() {
  const input = document.getElementById('chatbot-input');
  input.addEventListener('keypress', handleChatInput);
  setTimeout(() => {
    addChatMessage("Hey! 👋 I'm your AI Assistant. Need help with your tasks?", false);
  }, 800);
}

function toggleChatbot() {
  const container = document.getElementById('chatbot-container');
  container.classList.toggle('hidden');
}

function addChatMessage(text, isUser = false) {
  const messagesDiv = document.getElementById('chatbot-messages');
  const messageDiv = document.createElement('div');
  messageDiv.className = `chat-message ${isUser ? 'message-user' : 'message-bot'}`;
  
  const bubble = document.createElement('div');
  bubble.className = 'message-bubble';
  bubble.textContent = text;
  
  messageDiv.appendChild(bubble);
  messagesDiv.appendChild(messageDiv);
  
  setTimeout(() => {
    messagesDiv.scrollTop = messagesDiv.scrollHeight;
  }, 0);
}

function sendChatMessage() {
  const input = document.getElementById('chatbot-input');
  const message = input.value.trim();
  
  if (message === '') return;
  
  addChatMessage(message, true);
  input.value = '';
  
  const lowerMessage = message.toLowerCase();
  let response = chatbotResponses.default;
  
  if (lowerMessage.includes('hello') || lowerMessage.includes('hi') || lowerMessage.includes('hey')) {
    response = chatbotResponses.hello;
  } else if (lowerMessage.includes('help')) {
    response = chatbotResponses.help;
  } else if (lowerMessage.includes('task') && !lowerMessage.includes('add')) {
    response = chatbotResponses.tasks;
  } else if (lowerMessage.includes('today')) {
    response = chatbotResponses.today;
  } else if (lowerMessage.includes('add')) {
    response = chatbotResponses.add;
  } else if (lowerMessage.includes('complete') || lowerMessage.includes('done')) {
    response = chatbotResponses.complete;
  } else if (lowerMessage.includes('dashboard') || lowerMessage.includes('stats')) {
    response = chatbotResponses.dashboard;
  } else if (lowerMessage.includes('motivat') || lowerMessage.includes('good')) {
    response = chatbotResponses.motivation;
  }
  
  setTimeout(() => {
    addChatMessage(response, false);
  }, 500);
}

function handleChatInput(event) {
  if (event.key === 'Enter') {
    sendChatMessage();
  }
}