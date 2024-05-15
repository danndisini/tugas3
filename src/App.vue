<template>
  <div class="todo-app">
    <h1>App To Do</h1>
    <form @submit.prevent="tambah" class="todo-form">
      <input v-model="newTodo" required placeholder="Ketik disini" class="todo-input">
      <button type="submit" style="background-color: rgba(255, 255, 255, 0.5);" class="todo-button">Tambah</button>
    </form>

    <ul class="todo-list">
      <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <div v-if="editingTodo === todo.id">
          <input v-model="editingText" @keyup.enter="updateTodo" class="edit-input">
          <button @click="updateTodo" style="background-color: rgba(255, 255, 255, 0.5);" class="simpan-button">Simpan</button>
          <button @click="cancelEdit" style="background-color: rgba(255, 255, 255, 0.5);" class="batal-button">Batal</button>
        </div>
        <div v-else>
          <input type="checkbox" v-model="todo.done">
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
          <button @click="edit(todo)"  style="background-color: rgba(255, 255, 255, 0.5);" class="edit-button">Edit</button>
          <button @click="hapus(todo)" style="background-color: rgba(255, 255, 255, 0.5);" class="delete-button">Hapus</button>
        </div>
      </li>
    </ul>

    <button @click="hideCompleted = !hideCompleted" style="background-color: rgba(255, 255, 255, 0.5);" class="toggle-completed">
      {{ hideCompleted ? 'Tampilkan Semua' : 'Sembunyikan' }}
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
    {id: id++, text: 'test', done: true},
    {id: id++, text: 'learn vue', done: true},
    {id: id++, text: 'test lagi', done: false}
])

const editingTodo = ref(null)
const editingText = ref('')

function tambah() {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ id: id++, text: newTodo.value, done: false })
    newTodo.value = ''
  }
}

function hapus(todo) {
  todos.value = todos.value.filter((t) => t.id !== todo.id)
}

function edit(todo) {
  editingTodo.value = todo.id
  editingText.value = todo.text
}

function updateTodo() {
  if (editingTodo.value !== null) {
    const todo = todos.value.find(t => t.id === editingTodo.value)
    if (todo && editingText.value.trim() !== '') {
      todo.text = editingText.value
      cancelEdit()
    }
  }
}

function cancelEdit() {
  editingTodo.value = null
  editingText.value = ''
}

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})
</script>

<style scoped>
.todo-app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  border: 2px solid rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.todo-form {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.todo-input, .edit-input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.todo-button, .simpan-button, .batal-button {
  padding: 10px;
  border: none;
  color: rgb(0, 0, 0);
  border-radius: 4px;
  cursor: pointer;
  margin-left: 10px;
}




.todo-list {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.todo-item .done {
  text-decoration: line-through;
  color: #999;
}

.edit-button, .delete-button {
  background: none;
  border: none;
  cursor: pointer;
}

.edit-button {
  padding: 10px;
  border: none;
  color: rgb(0, 0, 0);
  border-radius: 4px;
  cursor: pointer;
  margin-left: 10px;
}

.delete-button {
  padding: 10px;
  border: none;
  color: rgb(0, 0, 0);
  border-radius: 4px;
  cursor: pointer;
  margin-left: 10px;
}

.toggle-completed {
  margin-top: 20px;
  padding: 10px;
  color: rgb(0, 0, 0);
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
