<template>
  <div class="bg-gray-200 mx-auto h-96 mt-60 p-10">
    <div class="w-full flex justify-center items-center">
      <input
        type="text"
        class="
          w-80
          h-12
          rounded-full
          placeholder-gray-600
          font-medium
          pl-6
          outline-none
          focus:ring-2 focus:ring-indigo-400
          text-xl
          shadow-2xl
        "
        v-model="inputValue"
        placeholder="Write new task..."
        @keydown.enter="addItem"
      />

      <button
        class="
          w-12
          h-12
          rounded-full
          bg-indigo-600
          text-white
          flex
          justify-center
          items-center
          text-4xl
          ml-4
        "
        @click="addItem"
      >
        +
      </button>
    </div>

    <ul class="w-full flex flex-col items-center mt-4 space-y-4">
      <li
        v-for="item in todoList"
        :key="item.id"
        class="
          w-10/12
          h-12
          bg-white
          rounded-lg
          flex
          justify-between
          items-center
          px-4
          shadow
        "
      >
        <input
          v-if="currentId === item.id"
          type="text"
          class="
            w-60
            h-10
            rounded-md
            font-medium
            pl-6
            outline-none
            ring-1 ring-indigo-400
          "
          v-model="editeValue"
          @keydown.enter="setItem"
        />
        <div v-else class="flex items-center">
          <input
            type="checkbox"
            :checked="item.checked"
            @change="checkedItem($event, item.id)"
            class="w-4 h-4 mr-2 text-red-500"
          />
          <p :class="item.checked ? 'text-gray line-through' : 'transition'">
            {{ item.msg }}
          </p>
        </div>

        <div class="flex space-x-2">
          <span
            v-if="currentId === item.id"
            class="cursor-pointer"
            @click="setItem"
            >✅</span
          >
          <span class="cursor-pointer" @click="editeItem(item.id, item.msg)"
            >🖋</span
          >
          <span class="cursor-pointer" @click="deleteItem(item.id)">❌</span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue';

interface ITodoItem {
  id: number;
  msg: string;
  checked: boolean;
  time: string;
}

const inputValue = ref('');
const todoList: ITodoItem[] = reactive([]);
const currentId = ref(0);
const editeValue = ref('');

const addItem = () => {
  const item: ITodoItem = {
    id: Date.now(),
    msg: inputValue.value,
    checked: false,
    time: new Date().toLocaleTimeString(),
  };

  todoList.unshift(item);
  inputValue.value = '';
};

const deleteItem = (id: number) => {
  todoList.splice(
    todoList.findIndex((item) => item.id === id),
    1
  );
};

const checkedItem = (event: Event, id: number) => {
  todoList[todoList.findIndex((item) => item.id === id)].checked = (
    event.target as HTMLInputElement
  ).checked;
};

const editeItem = (id: number, msg: string) => {
  currentId.value = id;
  editeValue.value = msg;
};

const setItem = () => {
  todoList[todoList.findIndex((item) => item.id === currentId.value)].msg =
    editeValue.value;
  currentId.value = 0;
};
</script>
