<!DOCTYPE html>
<html lang="uk">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ToDo List</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      margin: 0;
      padding: 20px;
    }
    .container {
      max-width: 500px;
      margin: auto;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    h1 {
      text-align: center;
    }
    input[type="text"] {
      width: 80%;
      padding: 10px;
      margin-right: 10px;
    }
    button {
      padding: 10px;
    }
    ul {
      list-style: none;
      padding: 0;
    }
    li {
      padding: 10px;
      background: #f9f9f9;
      margin-top: 5px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-radius: 4px;
    }
    .completed {
      text-decoration: line-through;
      color: gray;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Список завдань</h1>
    <div>
      <input type="text" id="taskInput" placeholder="Нове завдання..." />
      <button onclick="addTask()">Додати</button>
    </div>
    <ul id="taskList"></ul>
  </div>

  <script>
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

    function saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    function renderTasks() {
      const taskList = document.getElementById('taskList');
      taskList.innerHTML = '';
      tasks.forEach((task, index) => {
        const li = document.createElement('li');
        li.className = task.completed ? 'completed' : '';
        li.innerHTML = `
          <span onclick="toggleTask(${index})">${task.text}</span>
          <button onclick="deleteTask(${index})">Видалити</button>
        `;
        taskList.appendChild(li);
      });
    }

    function addTask() {
      const input = document.getElementById('taskInput');
      const text = input.value.trim();
      if (text) {
        tasks.push({ text, completed: false });
        saveTasks();
        renderTasks();
        input.value = '';
      }
    }

    function toggleTask(index) {
      tasks[index].completed = !tasks[index].completed;
      saveTasks();
      renderTasks();
    }

    function deleteTask(index) {
      tasks.splice(index, 1);
      saveTasks();
      renderTasks();
    }

    renderTasks();
  </script>
</body>
</html>
