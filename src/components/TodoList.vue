<template>
  <section class="main">
    <!-- 是否全选 -->
    <input id="toggle-all" class="toggle-all" type="checkbox" />
    <label for="toggle-all"></label>
    <!-- Todo列表 -->
    <ul class="todo-list">
      <li class="todo">
        <div class="view">
          <input class="toggle" type="checkbox" />
          <label @dblclick="editTodo(todo)">This will be done today</label>
          <button class="destroy" @click="removeTodo(todo)"></button>
        </div>
        <input class="edit" type="text" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)"
          @keyup.esc="cancelEdit(todo)" />
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  name: 'TodoList'
}
</script>

<style scoped lang="less">
.main {
  z-index: 2;
  position: relative;
  border-top: 1px solid #e6e6e6;
}

.toggle-all {
  width: 1px;
  height: 1px;
  border: none;
  /* Mobile Safari */
  opacity: 0;
  position: absolute;
  right: 100%;
  bottom: 100%;

  +label {
    width: 60px;
    height: 34px;
    font-size: 0;
    position: absolute;
    top: -52px;
    left: -13px;
    -webkit-transform: rotate(90deg);
    transform: rotate(90deg);
  }

  +label:before {
    content: '❯';
    font-size: 22px;
    color: #e6e6e6;
    padding: 10px 27px 10px 27px;
  }
}

.toggle-all:checked+label:before {
  color: #737373;
}

.todo-list {
  margin: 0;
  padding: 0;
  list-style: none;

  li {
    position: relative;
    font-size: 24px;
    border-bottom: 1px solid #ededed;

    .toggle {
      width: 40px;
      opacity: 0;
      text-align: center;
      /* auto, since non-WebKit browsers doesn't support input styling */
      height: auto;
      position: absolute;
      top: 0;
      bottom: 0;
      margin: auto 0;
      border: none;
      /* Mobile Safari */
      -webkit-appearance: none;
      appearance: none;

      +label {
        /*
      		Firefox requires `#` to be escaped - https://bugzilla.mozilla.org/show_bug.cgi?id=922433
      		IE and Edge requires *everything* to be escaped to render, so we do that instead of just the `#` - https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/7157459/
      	*/
        background-image: url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23ededed%22%20stroke-width%3D%223%22/%3E%3C/svg%3E');
        background-repeat: no-repeat;
        background-position: center left;
      }
    }

    .toggle:checked+label {
      background-image: url('data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A//www.w3.org/2000/svg%22%20width%3D%2240%22%20height%3D%2240%22%20viewBox%3D%22-10%20-18%20100%20135%22%3E%3Ccircle%20cx%3D%2250%22%20cy%3D%2250%22%20r%3D%2250%22%20fill%3D%22none%22%20stroke%3D%22%23bddad5%22%20stroke-width%3D%223%22/%3E%3Cpath%20fill%3D%22%235dc2af%22%20d%3D%22M72%2025L42%2071%2027%2056l-4%204%2020%2020%2034-52z%22/%3E%3C/svg%3E');
    }

    label {
      display: block;
      line-height: 1.2;
      word-break: break-all;
      padding: 15px 15px 15px 60px;
      transition: color 0.4s;
    }

    .destroy {
      display: none;
      position: absolute;
      top: 0;
      right: 10px;
      bottom: 0;
      width: 40px;
      height: 40px;
      margin: auto 0;
      font-size: 30px;
      color: #cc9a9a;
      margin-bottom: 11px;
      transition: color 0.2s ease-out;

      &:hover {
        color: #af5b5e;
      }

      &:after {
        content: '×';
      }
    }

    .edit {
      display: none;
    }
  }

  li:hover .destroy {
    display: block;
  }

  li:last-child {
    border-bottom: none;
  }

  li.editing {
    border-bottom: none;
    padding: 0;

    &:last-child {
      margin-bottom: -1px;
    }

    .edit {
      display: block;
      width: calc(100% - 43px);
      padding: 12px 16px;
      margin: 0 0 0 43px;
    }

    .view {
      display: none;
    }
  }

  li.completed label {
    color: #d9d9d9;
    text-decoration: line-through;
  }
}

.edit {
  position: relative;
  margin: 0;
  width: 100%;
  font-size: 24px;
  font-family: inherit;
  font-weight: inherit;
  line-height: 1.4em;
  border: 0;
  color: inherit;
  padding: 6px;
  border: 1px solid #999;
  box-shadow: inset 0 -1px 5px 0 rgba(0, 0, 0, 0.2);
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
