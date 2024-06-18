<template>
    <div class="todo-list">
      <h1>ToDo List</h1>
      <div class="input-group">
        <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new todo" />
        <select v-model="newPriority">
          <option value="1">Low</option>
          <option value="2">Medium</option>
          <option value="3">High</option>
        </select>
        <button @click="addTodo">Add</button>
      </div>
      <div class="filters">
        <span>{{ incompleteCount }} items left</span>
        <button @click="filter = 'all'">All</button>
        <button @click="filter = 'active'">Active</button>
        <button @click="filter = 'completed'">Completed</button>
        <button @click="toggleAll">Toggle All</button>
      </div>
      <ul>
        <li v-for="(todo, index) in filteredTodos" :key="index" :class="{ completed: todo.completed }">
          <div class="todo-item">
            <input type="checkbox" v-model="todo.completed" />
            <span v-if="!todo.editing" @dblclick="editTodo(todo)">
              {{ todo.text }} - {{ priorityText(todo.priority) }}
            </span>
            <input v-else v-model="todo.text" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" />
          </div>
          <button class="remove" @click="removeTodo(index)">Remove</button>
        </li>
      </ul>
      <button class="clear-completed" @click="clearCompleted">Clear Completed</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        newTodo: '',
        newPriority: '1',
        filter: 'all',
        todos: JSON.parse(localStorage.getItem('todos')) || []
      };
    },
    computed: {
      filteredTodos() {
        if (this.filter === 'active') {
          return this.todos.filter(todo => !todo.completed);
        } else if (this.filter === 'completed') {
          return this.todos.filter(todo => todo.completed);
        }
        return this.todos;
      },
      sortedTodos() {
        return this.todos.slice().sort((a, b) => b.priority - a.priority);
      },
      incompleteCount() {
        return this.todos.filter(todo => !todo.completed).length;
      }
    },
    methods: {
      addTodo() {
        if (this.newTodo.trim()) {
          this.todos.push({ text: this.newTodo, completed: false, priority: this.newPriority, editing: false });
          this.newTodo = '';
          this.newPriority = '1';
          this.saveTodos();
        }
      },
      removeTodo(index) {
        this.todos.splice(index, 1);
        this.saveTodos();
      },
      clearCompleted() {
        this.todos = this.todos.filter(todo => !todo.completed);
        this.saveTodos();
      },
      editTodo(todo) {
        todo.editing = true;
      },
      doneEdit(todo) {
        todo.editing = false;
        this.saveTodos();
      },
      toggleAll() {
        const allCompleted = this.todos.every(todo => todo.completed);
        this.todos.forEach(todo => (todo.completed = !allCompleted));
        this.saveTodos();
      },
      saveTodos() {
        localStorage.setItem('todos', JSON.stringify(this.todos));
      },
      priorityText(priority) {
        switch (priority) {
          case '1':
            return 'Low';
          case '2':
            return 'Medium';
          case '3':
            return 'High';
          default:
            return 'Low';
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .todo-list {
    max-width: 500px;
    margin: 50px auto;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    background-color: #f9f9f9;
  }
  
  h1 {
    text-align: center;
    color: #333;
    margin-bottom: 20px;
  }
  
  .input-group {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  
  input[type="text"] {
    flex: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px 0 0 5px;
  }
  
  select {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 0;
  }
  
  button {
    padding: 10px 20px;
    border: none;
    background-color: #42b983;
    color: white;
    cursor: pointer;
    border-radius: 0 5px 5px 0;
  }
  
  button:hover {
    background-color: #369f73;
  }
  
  .filters {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
  
  .filters button {
    background-color: #42b983;
    border: none;
    color: white;
    cursor: pointer;
    padding: 10px;
    border-radius: 5px;
    margin: 0 5px;
  }
  
  .filters button:hover {
    background-color: #369f73;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

li:last-child {
  border-bottom: none;
}

.todo-item {
  display: flex;
  align-items: center;
}

.todo-item input[type="checkbox"] {
  margin-right: 10px;
}

.completed span {
  text-decoration: line-through;
  color: #999;
}

.remove {
  background-color: #ff6b6b;
  border: none;
  color: white;
  cursor: pointer;
  padding: 5px 10px;
  border-radius: 5px;
}

.remove:hover {
  background-color: #e05a5a;
}

.clear-completed {
  margin-top: 20px;
  background-color: #ff6b6b;
  border: none;
  color: white;
  cursor: pointer;
  padding: 10px 20px;
  border-radius: 5px;
}

.clear-completed:hover {
  background-color: #e05a5a;
}
</style>
