<template>
  <div class="container">
    <div class="header">
      <ul>
        <li @click="selectedMenu = 'Todos'" :class="{ active: selectedMenu === 'Todos' }">Todos</li>
        <li @click="selectedMenu = 'Post'" :class="{ active: selectedMenu === 'Post' }">Post</li>
      </ul>
    </div>
    <div class="konten">
      <h1 v-if="selectedMenu === 'Todos'">To-Do List My Course</h1>
      <hr v-if="selectedMenu === 'Todos'" />
      <form v-if="selectedMenu === 'Todos'" @submit.prevent="addTodo">
        <input class="input" v-model="newTodo" />
        <button>Add Course</button>
      </form>
      <ul v-if="selectedMenu === 'Todos'">
        <li v-for="todo in filteredTodos" :key="todo.id">
          <div class="list">
            <input type="checkbox" v-model="todo.done" />
            <span :class="{ done: todo.done }" v-if="editingTodo !== todo">{{ todo.text }}</span>
            <input v-else v-model="editedTodoText" @keyup.enter="saveEdit(todo)" @keyup.esc="cancelEdit()" />
            <button @click="removeTodo(todo)">X</button>
            <button v-if="editingTodo !== todo" @click="editTodo(todo)">Edit</button>
            <button v-else @click="saveEdit(todo)">Save</button>
            <button v-if="editingTodo === todo" @click="cancelEdit()">Cancel</button>
          </div>
        </li>
      </ul>
      <button v-if="selectedMenu === 'Todos'" @click="hideCompleted = !hideCompleted">
        {{ hideCompleted ? 'Show all' : 'Hide completed' }}
      </button>
      <div v-if="selectedMenu === 'Post'">
        <label for="userSelect">Select User:</label>
        <select id="userSelect" v-model="selectedUser">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
        <ul v-if="selectedUser">
          <li v-for="post in userPosts" :key="post.id">
            {{ post.title }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedMenu: 'Todos', // Default menu selection
      newTodo: '',
      hideCompleted: false,
      todos: [
        { id: 1, text: 'Basis Data', done: true },
        { id: 2, text: 'jaringan Komputer', done: true },
        { id: 3, text: 'Struktur Data', done: false }
      ],
      editingTodo: null,
      editedTodoText: '',
      users: [], // Placeholder for users data
      selectedUser: null, // Selected user ID for posts
      userPosts: [] // Placeholder for user posts data
    };
  },
  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.todos.filter((t) => !t.done)
        : this.todos;
    }
  },
  methods: {
    addTodo() {
      this.todos.push({ id: this.todos.length + 1, text: this.newTodo, done: false });
      this.newTodo = '';
    },
    removeTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo);
    },
    editTodo(todo) {
      this.editingTodo = todo;
      this.editedTodoText = todo.text;
    },
    cancelEdit() {
      this.editingTodo = null;
      this.editedTodoText = '';
    },
    saveEdit(todo) {
      todo.text = this.editedTodoText;
      this.cancelEdit();
    },
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => this.users = data)
        .catch(error => console.error('Error fetching users:', error));
    },
    fetchUserPosts(userId) {
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`)
        .then(response => response.json())
        .then(data => this.userPosts = data)
        .catch(error => console.error(`Error fetching posts for user ${userId}:`, error));
    }
  },
  watch: {
    selectedUser(newVal) {
      if (newVal) {
        this.fetchUserPosts(newVal);
      }
    }
  },
  created() {
    this.fetchUsers();
  }
};
</script>

<style>
.header {
  background-color: #f2f2f2;
  padding: 10px;
}

.header ul {
  list-style-type: none;
  padding: 0;
}

.header ul li {
  display: inline-block;
  margin-right: 10px;
  cursor: pointer;
}

.header ul li.active {
  font-weight: bold;
}

.konten {
  margin-top: 20px;
}

.done {
  text-decoration: line-through;
}

button {
  color: rgb(181, 21, 40);
  background-color: pink;
  border: 2px solid rgb(146, 24, 57);
}
</style>
