<script setup lang="ts">
import { ref } from "vue";

type TodoType = {
  text: string;
  done: boolean;
};

const items = ref<TodoType[]>(
  JSON.parse(localStorage.getItem("items") || "[]")
);
const inputValue = ref<string>("");
const addItem = () => {
  const item = {
    text: inputValue.value,
    done: false,
  };
  items.value.push(item);
  localStorage.setItem("items", JSON.stringify(items.value));
  inputValue.value = "";
};
const deleteItem = (todoText) => {
  items.value.map((item, index) =>
    item.text === todoText ? items.value.splice(index, 1) : null
  );
  localStorage.setItem("items", JSON.stringify(items.value));
};
const doneItem = (todoText) => {
  items.value.map((item) =>
    item.text === todoText ? (item.done = true) : null
  );
  localStorage.setItem("items", JSON.stringify(items.value));
};
</script>
<template>
  <div class="h-100 w-full flex items-center justify-center font-sans">
    <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-xl">
      <div class="mb-4">
        <h1 class="text-gray-800">Todo List</h1>
        <div class="flex mt-4">
          <input
            v-model="inputValue"
            class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-gray-600"
            placeholder="Add Todo"
          />
          <button
            @click="addItem()"
            :disable="inputValue.length === 0"
            class="flex-no-shrink p-2 border-2 rounded text-teal-300 border-teal-300 hover:text-white hover:bg-teal-300"
          >
            Add
          </button>
        </div>
      </div>
      <div>
        <div
          v-for="(item, index) in items"
          :key="index"
          class="flex mb-4 items-center"
        >
          <p
            :class="item.done ? 'line-through text-green-400' : 'text-gray-800'"
            class="w-full"
          >
            {{ item.text }}
          </p>
          <button
            @click="doneItem(item.text)"
            class="flex-no-shrink p-2 ml-4 mr-2 border-2 rounded hover:text-white text-green-400 border-green-400 hover:bg-green-400"
          >
            Done
          </button>
          <button
            @click="deleteItem(item.text)"
            class="flex-no-shrink p-2 ml-2 border-2 rounded text-red-400 border-red-400 hover:text-white hover:bg-red-400"
          >
            Remove
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
