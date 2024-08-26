<template>
  <main class="flex max-w-screen justify-center">
    <div class="container border xl:w-[40%] mt-2 md:w-[60%] lg:w-[50%] rounded-md shadow-sm p-2 md:p-4">
      <div class="flex items-center mb-2 justify-between">
        <div class="flex items-center">
          <img
            class="w-[30px] md:w-[60px] h-[30px] md:h-[60px] object-cover"
            src="/logo.png"
            alt="logo"
          />
          <h1 class="text-xl">Todo List</h1>
        </div>
        <h1 class="font-bold">{{ date }}</h1>
      </div>

      <div
        class="mb-5 flex border px-3 py-1 overflow-hidden items-center justify-between w-full rounded-full"
      >
        <input
          v-model="newTodo"
          class="outline-none border-none w-[80%] p-2 md:p-3"
          placeholder="Add a new todo"
          @keyup.enter="addTodo"
        />
        <button
          class="py-1 md:py-2 hover:bg-green-500 bg-green-700 flex justify-center items-center hover:font-semibold hover:text-white duration-200 active:scale-95 px-3 rounded-full mb-[5px] w-[20%] mt-2"
          @click="addTodo"
        >
          Add
        </button>
      </div>

      <ul>
        <h1 class="font-semibold text-sm">Todo List:</h1>
        <li
          v-for="(todo, index) in todos"
          :key="index"
          :class="`mb-2 p-3 w-full border border-green-100 rounded-lg flex justify-between items-center ${todo.completed ? 'completedParent' : ''}`"
        >
          <span :class="`w-full font-semibold ${todo.completed ? 'completed' : ''}`">
            {{ todo.text }}
          </span>
          <div class="flex">
            <button class="border-none ml-2" @click="toggleComplete(index)">
              <div
                class="p-2 border rounded-lg flex justify-center items-center hover:bg-green-500 hover:border-green-600 hover:text-white duration-200 text-green-500"
              >
                <Icon
                  v-if="todo.completed"
                  class="border-none outline-none"
                  height="20"
                  icon="mdi:undo-variant"
                />
                <!-- <iconify-icon icon="mdi:undo-variant"></iconify-icon> -->
                <Icon
                  v-else
                  class="border-none"
                  height="20"
                  icon="mdi:done"
                />
              </div>
            </button>
            <button class="ml-2 border-none" @click="confirmDelete(index)">
              <div
                class="p-2 border rounded-lg flex justify-center items-center hover:bg-green-500 hover:border-green-600 hover:text-white duration-200 text-green-500"
              >
                <Icon
                  class="border-none"
                  height="20"
                  icon="mdi:delete"
                />
              </div>
            </button>
          </div>
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
import { ref, onMounted } from 'vue';
import Icon from '@iconify/vue';

export default {
  components: {
    Icon,
  },
  setup() {
    const todos = ref([]);
    const newTodo = ref('');
    const date = ref('');

    onMounted(() => {
      date.value = new Date().toLocaleDateString();
    });

    function addTodo() {
      if (newTodo.value.trim()) {
        todos.value.push({ text: newTodo.value.trim(), completed: false });
        newTodo.value = '';
      }
    }

    function confirmDelete(index) {
      if (confirm('Are you sure you want to delete this todo?')) {
        todos.value.splice(index, 1);
      }
    }

    function toggleComplete(index) {
      todos.value[index].completed = !todos.value[index].completed;
    }

    return {
      todos,
      newTodo,
      date,
      addTodo,
      confirmDelete,
      toggleComplete,
    };
  },
};
</script>


<style scoped>
.completed {
  text-decoration: line-through;
  color: gray;
}

.completedParent {
  opacity: 0.3;
}
</style>
