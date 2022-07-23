<template>
  <header class="header">
    <h1>todos</h1>
    <input class="new-todo" autofocus autocomplete="off" placeholder="What needs to be done?" @keyup.enter="addTodo" />
  </header>
</template>

<script>
export default {
  name: 'TodoHeader',

  props: {
    msg: String
  }
}
</script>

<style scoped lang="less">
.new-todo {
  position: relative;
  width: 100%;
  margin: 0;
  padding: 16px 16px 16px 60px;
  border: none;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  color: inherit;
  background: rgba(0, 0, 0, 0.003);
  box-sizing: border-box;
  box-shadow: inset 0 -2px 1px rgba(0, 0, 0, 0.03);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
