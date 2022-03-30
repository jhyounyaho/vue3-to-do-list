<template>
  <div class="container">
    <h2>To-Do List</h2>
    <input 
      class="form-control mr-2"
      type="text" 
      v-model="searchText"
      placeholder="Search"
    />
    <hr />
    <button @click="count++">Add One</button>
    <TodoSimpleForm @add-todo="addTodo" />

    <div v-if="!filterTodos.length">
      검색 결과가 없습니다.
    </div>
    <div v-if="!todos.length">
      추가된 Todo가 없습니다.
    </div>
    <!-- <TodoList :todos="todos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" /> -->
    <TodoList :todos="filterTodos" @toggle-todo="toggleTodo" @delete-todo="deleteTodo" />
  </div>
</template>

<script>
import { ref, computed } from 'vue';
import TodoSimpleForm from './components/TodoSimpleForm.vue';
import TodoList from './components/TodoList.vue';

export default {
  components: {
    TodoSimpleForm,
    TodoList,
  },
  setup() {
    // TODO ref, reactive 공부하기 
    const todos = ref([]);

    const addTodo = (todo) => {
      todos.value.push(todo)
    }

    const deleteTodo = (index) => {
      todos.value.splice(index, 1);
    }

    const toggleTodo = (index) => {
      todos.value[index].completed = !todos.value[index].completed 
    }

    const searchText = ref('');
    const filterTodos = computed(() => {
      if (searchText.value) {
        return todos.value.filter(todo => {
          return todo.subject.includes(searchText.value);
        })
      }
      return todos.value;
    })

    return {
      todos,
      addTodo,
      deleteTodo,
      toggleTodo,
      searchText,
      filterTodos,
    }
  }
}
</script>

<style>
  .todo {
    color: gray;
    text-decoration: line-through;
  }
</style>
