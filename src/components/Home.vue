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
        <!-- ここに <tr> で ToDo の要素を1行づつ繰り返し表示 -->
        <tr v-for="item in todos" v-bind:key="item.id">
          <th>{{ item.id }}</th>
          <td>{{ item.comment }}</td>
          <td class="state">
            <!-- 状態変更ボタンのモック -->
            <button v-on:click="doChangeState(item)">
              {{ item.state }}
            </button>
          </td>
          <td class="button">
            <!-- 削除ボタンのモック -->
            <button v-on:click.ctrl="doRemove(item)">削除</button>
            <button>削除</button>
          </td>
        </tr>
      </tbody>
    </table>

    <h2>新しい作業の追加</h2>
    <form class="add-form" v-on:submit.prevent="doAdd">
      <!-- コメント入力フォーム -->
      コメント <input type="text" ref="comment" />
      <!-- 追加ボタンのモック -->
      <button type="submit">追加</button>
    </form>
  </div>
</template>

<script lang="ts">
/* eslint-disable @typescript-eslint/no-explicit-any*/
import { defineComponent } from "vue";

interface Itodo {
  id: number;
  comment: string;
  state: number;
}

// https://jp.vuejs.org/v2/examples/todomvc.html
var STORAGE_KEY = "todos-vuejs-demo";
var todoStorage = {
  uid: 0,

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
    todos: [] as Itodo[],
  }),

  created() {
    // インスタンス作成時に自動的に fetch() する
    this.todos = todoStorage.fetch();
  },

  methods: {
    // ToDo 追加の処理
    // doAdd: function (event: any, value: any) {
    doAdd: function () {
      // ref で名前を付けておいた要素を参照
      var comment: any = this.$refs.comment;
      // 入力がなければ何もしないで return
      if (!comment.value.length) {
        return;
      }
      // { 新しいID, コメント, 作業状態 }
      // というオブジェクトを現在の todos リストへ push
      // 作業状態「state」はデフォルト「作業中=0」で作成
      this.todos.push({
        id: todoStorage.uid++,
        comment: comment.value,
        state: 0,
      });
      // フォーム要素を空にする
      comment.value = "";
    },

    // 状態変更の処理
    doChangeState: function (item: Itodo) {
      item.state = item.state ? 0 : 1;
    },

    // 削除の処理
    doRemove: function (item: Itodo) {
      var index = this.todos.indexOf(item);
      this.todos.splice(index, 1);
    },
  },

  watch: {
    // オプションを使う場合はオブジェクト形式にする
    todos: {
      // 引数はウォッチしているプロパティの変更後の値
      handler: function (todos: Itodo[]) {
        todoStorage.save(todos);
      },
      // deep オプションでネストしているデータも監視できる
      deep: true,
    },
  },
});
</script>

<style lang="scss" scoped></style>
