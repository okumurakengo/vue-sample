<template>
  <div id="app" class="container">
    <h1>
      <button @click="purge">Purge</button>
      My Todos
      <span class="info">({{ remaining.length }}/{{ todos.length }})</span>
    </h1>
    <ul>
      <li v-for="(todo, index) in todos">
        <input type="checkbox" v-model="todo.isDone" />
        <span :class="{done: todo.isDone}">{{ todo.title }}</span>
        <span @click="deleteItem(index)" class="command">[x]</span>
      </li>
      <li v-show="!todos.length">Nothing to do, yay!</li>
    </ul>
    <form @submit.prevent="addItem">
      <input type="text" v-model="newItem" />
      <input type="submit" value="Add" />
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newItem: "",
      todos: []
    };
  },
  watch: {
    todos: {
      handler() {
        localStorage.setItem("todos", JSON.stringify(this.todos));
      },
      deep: true
    }
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos")) || [];
  },
  methods: {
    addItem() {
      const item = {
        title: this.newItem,
        isDone: false
      };
      this.todos.push(item);
      this.newItem = "";
    },
    deleteItem(index) {
      if (confirm("are you sure?")) {
        this.todos.splice(index, 1);
      }
    },
    purge() {
      if (!confirm("delete finished?")) {
        return;
      }
      this.todos = this.remaining;
    }
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => {
        return !todo.isDone;
      });
    }
  }
};
</script>

<style>
body {
  font-size: 16px;
  font-family: Verdana, sans-serif;
}

.container {
  width: 300px;
  margin: auto;
}

#app h1 {
  font-size: 16px;
  border-bottom: 1px solid #ddd;
  padding: 16px 0;
}

#app li {
  line-height: 1.5;
}

#app input[type="text"] {
  padding: 2px;
}

.command {
  font-size: 12px;
  cursor: pointer;
  color: #08c;
}

#app ul {
  padding: 0;
  list-style: none;
}

#app li > span.done {
  text-decoration: line-through;
  color: #bbb;
}

.info {
  color: #bbb;
  font-size: 12px;
  font-weight: normal;
}

#app h1 > button {
  float: right;
}
</style>

