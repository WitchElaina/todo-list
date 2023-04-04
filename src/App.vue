<script setup>
  import {ref} from "vue";

  let todoList = ref([]);
  const newTodoName = ref('');
  const todoId = ref(0);

  const addTodo = () => {
    todoList.value.push({
      id: todoId.value++,
      name: newTodoName.value || 'New Todo',
      done: false
    });
    newTodoName.value = '';
  };

  const removeTodo = (id) => {
    todoList.value.splice(todoList.value.findIndex(todo => todo.id === id), 1);
  };


  const toggleTodo = (id) => {
    todoList.value = todoList.value.map(todo => {
      if (todo.id === id) {
        todo.done = !todo.done;
      }
      return todo;
    });
  };


</script>

<template>
  <top>
      <up></up>
      <header>
          <h1>Todos</h1>
          <div id="todoStatus">
              <div id="totalComp">
                  <span id="totalCompleted" class="countData">{{todoList.filter(todo => todo.done).length}}</span><span class="countName">Comp.</span>
              </div>
              <div id="totalNum">
                  <span id="totalTodos" class="countData">{{todoList.length}}</span><span class="countName">Total</span>
              </div>
          </div>
      </header>
      <main>
          <div id="todoInput">
              <div class="listItem">
                  <input type="text" v-model="newTodoName" placeholder="New..." @keydown.enter="addTodo">
              </div>
          </div>
          <div id="todoList" v-for="todo in todoList" :key="todo.id">
              <div class="listItem">
                  <div class="todoName" @click="toggleTodo(todo.id)">{{todo.name}}</div>
                  <button @click="removeTodo(todo.id)">×</button>
              </div>
          </div>

      </main>
  </top>

</template>