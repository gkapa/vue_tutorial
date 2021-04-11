<template>
  <div id="Home">
    <table>
      <!-- テーブルヘッダー -->
      <thead>
        <tr>
          <th class="id">ID</th>
          <th class="comment">コメント</th>
          <th class="state">状態</th>
          <th class="button">-</th>
        </tr>
      </thead>
      <tbody>
        <!-- [1] ここに <tr> で ToDo の要素を1行づつ繰り返し表示したい -->
      </tbody>
    </table>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface Itodo {
  id: number;
  comment: string;
  state: number;
}

// https://jp.vuejs.org/v2/examples/todomvc.html
var STORAGE_KEY = "todos-vuejs-demo";
var todoStorage = {
  uid: null,

  fetch: function () {
    var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || "[]");
    todos.forEach(function (todo: Itodo, index: number) {
      todo.id = index;
    });
    todoStorage.uid = todos.length;
    return todos;
  },
  save: function (todos: Itodo[]) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos));
  },
};

export default defineComponent({
  el: "#app",
  data: () => ({
    data: {
      // 使用するデータ
    },
    methods: {
      // 使用するメソッド
    },
  }),
});
</script>

<style lang="scss" scoped></style>
