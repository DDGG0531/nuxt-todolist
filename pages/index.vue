<template>
  <div class="mx-auto text-white bg-white h-screen">
    <div class="text-center p-5 bg-red-300">
      Hello Jim, Today is {{ dateStr }}
      <br />
      {{ `api result: ${apiResult}` }}
    </div>

    <div class="py-5 flex">
      <button
        @click="addTodo"
        class="
          ml-auto
          mr-1
          flex-shrink-0
          bg-purple-600
          text-white text-base
          font-semibold
          py-2
          px-4
          rounded-lg
          shadow-md
          hover:bg-purple-700
          focus:outline-none
          focus:ring-2
          focus:ring-purple-500
          focus:ring-offset-2
          focus:ring-offset-purple-200
        "
      >
        增加事項
      </button>
    </div>

    <!-- todoList  -->
    <div
      v-for="(todo, index) in todoList"
      :key="todo.id"
      class="flex items-center p-5 mb-5 bg-green-300 overflow-auto"
    >
      <!-- Checkbox  -->
      <label class="inline-flex items-center">
        <input
          type="checkbox"
          class="form-checkbox h-8 w-8 rounded-lg text-green-600"
          role="button"
          v-model="todo.checked"
        />
      </label>
      <!-- Name  -->
      <input
        class="
          form-input
          mr-5
          ml-5
          appearance-none
          border border-transparent
          py-2
          px-4
          bg-white
          text-gray-700
          placeholder-gray-400
          shadow-md
          rounded-lg
          text-base
          focus:outline-none
          focus:ring-2 focus:ring-green-600
          focus:border-transparent
        "
        v-model="todo.name"
        placeholder="輸入代辦事項"
      />
      <!-- Remove  -->
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-8 w-8 ml-auto flex-shrink-0 hover:shadow-md rounded-lg"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        role="button"
        @click="removeTodo(index)"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M15 12H9m12 0a9 9 0 11-18 0 9 9 0 0118 0z"
        />
      </svg>
    </div>
  </div>
</template>

<script>
import { format } from "date-fns";
import { v4 as uuidv4 } from "uuid";

const addNewTodo = () => {
  return {
    checked: false,
    name: "",
    id: uuidv4(),
  };
};

export default {
  name: "Main",
  data: () => {
    return {
      apiResult: "未完成",
      todoList: [addNewTodo()],
      dateStr: format(new Date(), "yyyy-MM-dd"),
    };
  },
  asyncData({ params }) {
    return fetch("https://jsonplaceholder.typicode.com/todos/1")
      .then((response) => response.json())
      .then((data) => {
        console.log("asyncData finished");
        return { apiResult: data.title };
      });
  },
  created() {
    console.log("created");
  },
  mounted() {
    console.log("mounted");
  },
  methods: {
    addTodo() {
      this.todoList.push(addNewTodo());
    },
    removeTodo(index) {
      this.todoList.splice(index, 1);
    },
  },
};
</script>

<style>
</style>