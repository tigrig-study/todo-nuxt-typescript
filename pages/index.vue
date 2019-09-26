<template>
  <div id="app">
    <h1>簡単な Todo リスト</h1>
    <!-- Todo リスト -->
    <ul>
      <li v-for="(todo, i) in todos" :key="i">
        <input type="checkbox" :checked="todo.done" @change="toggle(todo)" />
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <button @click="remove(todo)">削除</button>
      </li>
      <!-- 新規 Todo 入力エリア -->
      <li>
        <input placeholder="Todo を入力" @keyup.enter="addTodo" />
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
// 下記、「OptionsAPI で記述」するために必要
import Vue from 'vue'

// Todo の型定義をインポート
import { Todo } from '~/models/Todo'
// Todo リストのストアモジュールをインポート
import { todosStore } from '~/store'

// OptionsAPI で記述
export default Vue.extend({
  computed: {
    todos(): Array<Todo> {
      // リスト（todos）を取得
      // ※ todosStore. と打つと、インテリセンス（入力補完機能）が働く
      return todosStore.todos
    }
  },
  methods: {
    // Todo の追加
    addTodo(e): void {
      todosStore.add(e.target.value)
      e.target.value = ''
    },
    // Todo の削除
    remove(todo: Todo) {
      todosStore.remove(todo)
    },
    // Todo の done（完了状態）切り替え
    toggle(todo: Todo) {
      todosStore.toggle(todo)
    }
  }
})
</script>

<style>
/* 完了状態の Todo には打消し線をつける */
.done {
  text-decoration: line-through;
}
</style>
