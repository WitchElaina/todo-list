<script setup>
import {computed, ref} from "vue";

  let todoList = ref([]);
  const newTodoName = ref('');
  const todoId = ref(0);

  const readLocalStorage = () => {
    const data = localStorage.getItem('todoList');
    if (data) {
      todoList.value = JSON.parse(data);
      todoId.value = todoList.value.length;
    }
  };

  const writeLocalStorage = () => {
    localStorage.setItem('todoList', JSON.stringify(todoList.value));
  };

  const addTodo = () => {
    todoList.value.push({
      id: todoId.value++,
      name: newTodoName.value || 'New Todo',
      done: false
    });
    newTodoName.value = '';
    writeLocalStorage();
  };

  const removeTodo = (id) => {
    todoList.value.splice(todoList.value.findIndex(todo => todo.id === id), 1);
    writeLocalStorage();
  };


  const toggleTodo = (id) => {
    todoList.value = todoList.value.map(todo => {
      if (todo.id === id) {
        todo.done = !todo.done;
      }
      return todo;
    });
    writeLocalStorage()
  };

  const completedTodoNum = computed(() => {
    return todoList.value.filter(todo => todo.done).length;
  });

  readLocalStorage();

</script>

<template>
  <top>
      <up></up>
      <header>
          <h1>Todos</h1>
          <div id="todoStatus">
              <div id="totalComp">
                  <span id="totalCompleted" class="countData">{{completedTodoNum}}</span><span class="countName">Comp.</span>
              </div>
              <div id="totalNum">
                  <span id="totalTodos" class="countData">{{todoList.length}}</span><span class="countName">Total</span>
              </div>
          </div>
      </header>
      <main>
          <div id="todoInput">
              <div class="listItem">
                  <input type="text" v-model="newTodoName" placeholder="Input here and return to add..." @keydown.enter="addTodo" @keydown.esc="removeTodo(todoList[0].id)">
              </div>
          </div>
          <div id="todoList" v-for="todo in todoList" :key="todo.id">
              <div class="listItem">
                  <div v-if="todo.done" class="todoNameDone" @click="toggleTodo(todo.id)">{{todo.name}}</div>
                  <div v-else class="todoName" @click="toggleTodo(todo.id)">{{todo.name}}</div>
                  <button @click="removeTodo(todo.id)">×</button>
              </div>
          </div>

      </main>
  </top>

</template>