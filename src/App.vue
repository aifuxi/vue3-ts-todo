<template>
  <div class="todo-list-wrapper">
    <div class="todo-list-option">
      <input type="text" v-model="inputValue" @keyup.enter="addTodoItem" />
      <button @click="addTodoItem">新增</button>
    </div>
    <input type="checkbox" @change="toggleAll" /><span>全选/全不选</span>
    <ul>
      <li
      :class="{'line-through': item.done}"
        v-for="item in todoList"
        :key="item.time.getTime()"
      >
        <input
          v-if="currentId == item.id"
          type="text"
          :value="currentValue"
          @change="handleEdit"
        />
        <span v-else>
          <input
            type="checkbox"
            :checked="item.done"
            @change="handleChange($event, item.id)"
          />
          <span>{{ item.content }}</span>
          <span>{{ item.time }}</span>
        </span>
        <button @click="deleteTodoItem(item.id)">删除</button>
        <button @click="editTodoItem(item.id, item.content)">编辑</button>
        <button v-if="currentId == item.id" @click="currentId = 0">确定</button>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive, Ref, ref, watch } from 'vue';

interface ITodoListItem {
  id: number;
  content: string;
  done: boolean;
  time: Date;
}

enum DoneStatus {
  all = 1,
  completed,
  uncompleted,
}

const inputValue = ref('');
const currentValue = ref('');
const currentId = ref(0);

let todoList: ITodoListItem[] = reactive([]);

function addTodoItem() {
  if (!inputValue.value) {
    return alert('请输入内容');
  }
  const item: ITodoListItem = {
    id: Date.now(),
    content: inputValue.value.trim(),
    done: false,
    time: new Date(),
  };

  todoList.push(item);
  inputValue.value = '';
}
function toggleAll(event: Event) {
  todoList.forEach((item) => {
    item.done = (event.target as HTMLInputElement).checked;
  });
}

function deleteTodoItem(id: number) {
  todoList.splice(
    todoList.findIndex((item) => item.id == id),
    1
  );
}

function editTodoItem(id: number, content: string) {
  console.log(id, content);
  currentValue.value = content;
  currentId.value = id;
}

function handleChange(event: Event, id: number) {
  const index = todoList.findIndex((item) => item.id == id);
  todoList[index].done = (event.target as HTMLInputElement).checked;
}

function handleEdit(event: Event) {
  todoList[todoList.findIndex((item) => item.id == currentId.value)].content = (
    event.target as HTMLInputElement
  ).value;
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
.line-through {
  text-decoration: line-through;
}
</style>
