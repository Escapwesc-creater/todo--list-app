# todo--list-app
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Simple To-Do List App</title>
<style>
  body {
    font-family: Arial, sans-serif;
    max-width: 600px;
    margin: 2rem auto;
    padding: 1rem;
    background: #f9f9f9;
  }
  h1 {
    text-align: center;
  }
  #newTask {
    width: 70%;
    padding: 0.5rem;
    font-size: 1rem;
  }
  #addTaskBtn {
    padding: 0.6rem 1rem;
    font-size: 1rem;
  }
  ul {
    list-style-type: none;
    padding-left: 0;
  }
  li {
    background: white;
    margin-bottom: 0.5rem;
    padding: 0.75rem 1rem;
    border-radius: 5px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }
  button.deleteBtn {
    background: #d9534f;
    border: none;
    color: white;
    padding: 0.4rem 0.8rem;
    border-radius: 5px;
    cursor: pointer;
  }
</style>
</head>
<body>
  <h1>To-Do List</h1>
  <input type="text" id="newTask" placeholder="Add a new task..." />
  <button id="addTaskBtn">Add</button>
  <ul id="taskList"></ul>

  <script>
    const taskInput = document.getElementById('newTask');
    const addTaskBtn = document.getElementById('addTaskBtn');
    const taskList = document.getElementById('taskList');

    // Retrieve tasks from local storage or initialize empty array
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

    function renderTasks() {
      taskList.innerHTML = '';
      tasks.forEach((task, index) => {
        const li = document.createElement('li');
        li.className = task.completed ? 'completed' : '';
        li.innerHTML = `
          <span onclick="toggleComplete(${index})">${task.name}</span>
          <button class="deleteBtn" onclick="deleteTask(${index})">Delete</button>
        `;
        taskList.appendChild(li);
      });
    }

    function addTask() {
      const taskName = taskInput.value.trim();
      if (taskName === '') {
        alert('Please enter a task');
        return;
      }
      tasks.push({ name: taskName, completed: false });
      taskInput.value = '';
      saveTasks();
      renderTasks();
    }

    function toggleComplete(index) {
      tasks[index].completed = !tasks[index].completed;
      saveTasks();
      renderTasks();
    }

    function deleteTask(index) {
      tasks.splice(index, 1);
      saveTasks();
      renderTasks();
    }

    function saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    addTaskBtn.addEventListener('click', addTask);
    taskInput.addEventListener('keypress', function(e) {
      if (e.key === 'Enter') addTask();
    });

    // Initial render
    renderTasks();
  </script>
</body>
</html>
