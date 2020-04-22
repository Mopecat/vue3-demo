/**
 * dev  : yangliu_11
 * desc : 说明
 * props:
 * emit :
 * slot :
 */
<template>
  <div class="todolist">
    <div class="todolist--title">TodoList</div>

    <input class="todolist__input--add" placeholder="请输入todo" type="text" v-model="state.content" />
    <button class="todolist__btn todolist__btn--add" @click="add">添加</button>
    <ul class="todolist__ul">
      <li class="todolist__li" v-for="(item,index) in state.todolist" :key="index">
        {{item.content}}
        <button
          class="todolist__btn todolist__btn--delete"
          @click="deleteTodo(index)"
        >删除</button>
      </li>
    </ul>
  </div>
</template>

<script>
import {
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted
} from "vue";
import { reactive } from "vue";
// const state = reactive({
//   count: 0
// });
// watchEffect(() => {
//   document.body.innerHTML = `count is ${state.count}`;
// });
export default {
  setup() {
    const state = reactive({
      content: "",
      todolist: JSON.parse(localStorage.getItem("todolist")) || []
    });

    console.log("setup");
    onBeforeMount(() => {
      console.log("beforeMount---------挂载前");
    });
    onMounted(() => {
      console.log("mounted----------挂载后");
    });
    onBeforeUpdate(() => {
      console.log("beforeUpdate--------更新前");
    });
    onUpdated(() => {
      localStorage.setItem("todolist", JSON.stringify(state.todolist));
      console.log("updated-----------更新后");
    });
    onBeforeUnmount(() => {
      console.log(state.todolist);
    });
    onUnmounted(() => {
      console.log("unmounted------------卸载后");
    });
    const { add, deleteTodo } = todoList(state);
    return {
      state,
      add,
      deleteTodo
    };
  }
};
function todoList(state) {
  function add() {
    state.todolist.push({ content: state.content });
    state.content = "";
  }
  function deleteTodo(index) {
    state.todolist.splice(index, 1);
  }
  return {
    add,
    deleteTodo
  };
}
</script>

<style  scoped>
.todolist--title {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 10px 0;
  font-size: 24px;
  background: #333333;
  width: 200px;
  color: #ffffff;
  padding: 10px;
  border-radius: 4px;
}
.todolist__input--add {
  border-radius: 4px;
  border: 1px solid #dddddd;
  height: 30px;
  box-sizing: border-box;
  width: 200px;
  text-indent: 4px;
}
.todolist__btn {
  margin-left: 10px;
  height: 30px;
  border: 1px solid #dddddd;
  border-radius: 4px;
  padding: 0 10px;
}
.todolist__btn--add:hover {
  background: #25b864;
  border-color: #25b864;
}
.todolist__btn--delete {
  background: #ca0c16;
  border-color: #ca0c16;
  color: #ffffff;
}
.todolist__ul {
  padding: 0;
}
.todolist__li {
  list-style-type: none;
  font-size: 18px;
  border: 1px dashed #dddddd;
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
  padding: 6px 10px;
  width: 50%;
}
.todolist__li:hover {
  background: #dddddd;
}
</style>
