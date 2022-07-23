<template>
  <!-- 主输入框 -->
  <input class="new-todo" autofocus autocomplete="off" placeholder="What needs to be done?" v-model.trim="newTodo" @keyup.enter="addTodo" />
  <!-- 列表和操作区 -->
  <section class="main" v-show="todos.length">
    <!-- 一键全选 -->
    <input id="toggle-all" class="toggle-all" type="checkbox" v-model="allDone" />
    <label for="toggle-all"></label>
    <!-- Todo列表 -->
    <ul class="todo-list">
      <li class="todo" v-for="todo in todos" :key="todo.id" :class="{ completed: todo.completed, editing: todo == editedTodo }">
        <div class="view">
          <input class="toggle" type="checkbox" />
          <label @dblclick="editTodo(todo)">{{ todo.title }}</label>
          <button class="destroy" @click="removeTodo(todo)"></button>
        </div>
        <input class="edit" type="text" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" />
      </li>
    </ul>
  </section>
  <footer class="footer" v-show="todos.length">
    <span class="todo-count">
      <strong>0</strong> left
    </span>
    <!-- 列表数据过滤器 -->
    <ul class="filters">
      <li>
        <a href="#/all">All</a>
      </li>
      <li>
        <a href="#/active">Active</a>
      </li>
      <li>
        <a href="#/completed">Completed</a>
      </li>
    </ul>
    <button class="clear-completed">Clear completed</button>
  </footer>
</template>

<script>
export default {
  name: 'TodoList',

  data() {
    return {
      todos: [],
      newTodo: null
    }
  },

  methods: {
    addTodo() {
      if (!this.newTodo) {
        return;
      }

      this.todos.push({
        id: Date.now(),
        completed: false,
        title: this.newTodo
      });

      this.newTodo = null;
    },

    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    }
  }
}
</script>

<style scoped lang="less">
.main {
  z-index: 2;
  position: relative;
  border-top: 1px solid #e6e6e6;
}

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
.footer {
  color: #777;
  padding: 10px 15px;
  height: 20px;
  text-align: center;
  border-top: 1px solid #e6e6e6;

  &:before {
    content: '';
    position: absolute;
    right: 0;
    bottom: 0;
    left: 0;
    height: 50px;
    overflow: hidden;
    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2), 0 8px 0 -3px #f6f6f6, 0 9px 1px -3px rgba(0, 0, 0, 0.2), 0 16px 0 -6px #f6f6f6, 0 17px 2px -6px rgba(0, 0, 0, 0.2);
  }
}

.todo-count {
  float: left;
  text-align: left;

  strong {
    font-weight: 300;
  }
}

.filters {
  margin: 0;
  padding: 0;
  list-style: none;
  position: absolute;
  right: 0;
  left: 0;

  li {
    display: inline;

    a {
      color: inherit;
      margin: 3px;
      padding: 3px 7px;
      text-decoration: none;
      border: 1px solid transparent;
      border-radius: 3px;

      &:hover {
        border-color: rgba(175, 47, 47, 0.1);
      }
    }

    a.selected {
      border-color: rgba(175, 47, 47, 0.2);
    }
  }
}

.clear-completed,
html .clear-completed:active {
  float: right;
  position: relative;
  line-height: 20px;
  text-decoration: none;
  cursor: pointer;
}

.clear-completed:hover {
  text-decoration: underline;
}
</style>
