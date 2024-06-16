<script setup lang="ts">
import { ref, computed } from 'vue';

interface TodoItem {
  id: number;
  text: string;
  completed: boolean;
}

const inputQuery = ref('');

const currentFilter = ref<'all' | 'active' | 'done'>('all');
const filterTodos = (filterType: 'all' | 'active' | 'done') => {
  currentFilter.value = filterType;
}
const todos = ref<TodoItem[]>([
  { id: 1, text: 'Buy groceries', completed: false },
  { id: 2, text: 'Walk the dog', completed: true },
  { id: 3, text: 'Read a book', completed: false },
]);

const addTask = ():void => {
  if (inputQuery.value.trim() === '') {
    return;
  }

  const newItem = {
    id: new Date().getTime(),
    text: inputQuery.value.trim(),
    completed: false
  };

  todos.value.push(newItem);
  clearInput();
}

const clearInput = ():void => {
  inputQuery.value = '';
}

const deleteTodo = (todoId: number):void => {
  todos.value = todos.value.filter(item => item.id !== todoId);
}

const changeStatus = (todoItem: TodoItem): void => {
  todoItem.completed = !todoItem.completed;
}

const completedTodos = computed(() => {
  return todos.value.filter(task => task.completed);
});

const activeTodos = computed(() => {
  return todos.value.filter(task => !task.completed);
});

const filteredTodos = computed(() => {
  if (currentFilter.value === 'active') {
    return activeTodos.value;
  } else if (currentFilter.value === 'done') {
    return completedTodos.value;
  }

  return todos.value;
});

</script>

<template>
  <section class="todo__section">
    <div class="container">

      <h1 class="title">Todos List</h1>

      <ul class="todo__nav">
        <li>
          <button 
            @click="filterTodos('all')"
            :class="{ 'active': currentFilter === 'all' }"
            >
            All
          </button>
        </li>
        <li>
          <button 
            @click="filterTodos('active')"
            :class="{ 'active': currentFilter === 'active' }"
          >
            Active
          </button>
        </li>
        <li>
          <button 
            @click="filterTodos('done')"
            :class="{ 'active': currentFilter === 'done' }"
          >
            Done
          </button>
        </li>
      </ul>

      <form class="todo__form" @submit.prevent="addTask">
        <input 
          class="todo__item-input"
          v-model="inputQuery"
          type="text" 
          placeholder="New task..."
          required
        >
        <button
          type="submit"
          class="todo__item-add"
        >
          Add task
        </button>
      </form>

      <ul class="todo__container" v-if="filteredTodos.length">
        <li class="todo__item" v-for="todo in filteredTodos" :key="todo.id">
          <button 
            class="todo__item-status" 
            :class="{ 'completed': todo.completed }"
            @click="changeStatus(todo)"
          >
          </button>
          <p 
            class="todo__item-text" 
            :class="{ 'completed': todo.completed }"
          >
            {{ todo.text }}
          </p>
          <button 
            class="todo__item-remove" 
            @click="deleteTodo(todo.id)"
          >
            🗑️
          </button>
        </li>
      </ul>
      <p v-else class="todo__empty">No tasks yet</p>

      <p class="todo__result">{{ activeTodos.length }} more to do, {{ completedTodos.length }} done</p>

    </div>
  </section>
</template>

<style>
.todo__section {
  padding: 40px 0;
}

.todo__container {
  display: flex;
  flex-direction: column-reverse;
  row-gap: 20px;
}

.todo__nav {
  display: flex;
  margin-bottom: 24px;
}
.todo__nav li {
  width: 33.33%;
}
.todo__nav li button {
  width: 100%;
  height: 40px;
  border: 2px solid transparent;
  border-radius: 12px;
  font-weight: 500;
  font-size: 18px;
  line-height: 24px;
  text-align: center;
  color: #213547;
  cursor: pointer;
  transition: all .4s ease;
}
.todo__nav li button:hover,
.todo__nav li button.active {
  color: #4CAF50;
}
.todo__nav li button.active {
  pointer-events: none;
  border-color: #4CAF50;
}

.todo__form {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 24px;
}
.todo__item-input {
  width: calc(100% - 160px);
}
.todo__item-add {
  display: inline-block;
  width: 140px;
  height: 40px;
  padding: 6px 32px;
  border-radius: 12px;
  font-weight: 600;
  font-size: 18px;
  line-height: 24px;
  color: #FFF;
  background: #4CAF50;
}
.todo__item-add:hover {
  background: #44d14a;
}

.todo__item {
  display: flex;
  align-items: center;
  padding: 16px;
  border-radius: 12px;
  border: 1px solid #9E9E9E;
}
.todo__item-status {
  position: relative;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: rgba(0, 0, 0, 0.05);
}
.todo__item-status::after {
  opacity: 0;
  position: absolute;
  content: '';
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  width: 10px;
  height: 10px;
  border-radius: 50%;
  background: #4CAF50;
  transition: all .4s ease;
}
.todo__item-status.completed::after {
  opacity: 1;
}
.todo__item-text {
  flex: 1;
  padding: 0 16px;
  margin: 0;
}
.todo__item-text.completed {
  text-decoration: line-through;
  color: #9e9e9e;
}
.todo__item-remove {
  font-size: 20px;
}

.todo__empty {
  font-weight: 500;
  font-size: 20px;
  line-height: 30px;
  text-align: center;
}

.todo__result {
  margin-top: 24px;
  text-align: center;
}
</style>