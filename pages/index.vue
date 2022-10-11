<template>
  <div class="my-4">
    <h1 class="title">NUXT2 TODO LIST</h1>
    <div v-if="todos.length">
      <ul class="todos">
      <li v-if="!isEdit" class="row w-100 justify-content-center">
        <input
          type="text"
          v-model="newTodo"
          placeholder="Enter todo's thing here..."
          class="col-md-6"
          @keyup.enter="addTodo"
        /><button @click="addTodo" class="btn btn-sm btn-success col col-md-3">
          Add
        </button>
      </li>
      <li v-else class="row w-100 justify-content-center">
        <input
          type="text"
          v-model="temp.content"
          placeholder="Edit todo's thing here..."
          class="col-md-6"
          @keyup.enter="updateTodo(temp.id)"
        /><button
          @click="updateTodo(temp.id)"
          class="btn btn-sm custom-btn-dark col"
        >
          Confirm</button
        ><button
          @click="updateTodo(temp.id, true)"
          class="btn btn-sm custom-btn-danger col"
        >
          Cancel
        </button>
      </li>
      <li class="row w-100 my-2">
        <button
          class="col-4 btn btn-sm custom-btn-tab"
          :class="{ active: tabName === 'all' }"
          @click="tabName = 'all'"
        >
          All
        </button>
        <button
          class="col-4 btn btn-sm custom-btn-tab"
          :class="{ active: tabName === 'undo' }"
          @click="tabName = 'undo'"
        >
          Undo
        </button>
        <button
          class="col-4 btn btn-sm custom-btn-tab"
          :class="{ active: tabName === 'done' }"
          @click="tabName = 'done'"
        >
          Done
        </button>
      </li>
      <li v-for="item in filterTodos" :key="item.id" class="todos-item">
        <div>
          <input
            type="checkbox"
            v-model="item.isDone"
            @click="changeDone(item)"
            :id="item.id"
          />
          <label
            :for="item.id"
            :class="{ 'text-decoration-line-through': item.isDone }"
          >
            {{ item.content }}</label
          >
        </div>
        <div>
          <button
            class="btn custom-btn-dark btn-sm mx-2"
            @click="editTodo(item)"
          >
            Edit
          </button>
          <button @click="delTodo(item)" class="btn custom-btn-danger btn-sm">
            Delete
          </button>
        </div>
      </li>
    </ul>
    <p class="text-center">🎉 Already finish {{ doneNumber }} thing !</p>
    </div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      temp: {},
      tabName: "all",
      todos: [],
      isEdit: false,
      url: "http://localhost:3002/todos",
      oldVal: "",
    };
  },
  computed: {
    doneNumber() {
      const arr = this.todos.filter((item) => item.isDone);
      console.log(arr);
      return arr.length;
    },
    filterTodos() {
      if (this.tabName == "all") {
        return this.todos;
      } else if (this.tabName == "undo") {
        return this.todos.filter((item) => !item.isDone);
      } else {
        return this.todos.filter((item) => item.isDone);
      }
    },
  },
  methods: {
    changeDone(item) {
      item.isDone = !item.isDone;
      fetch(`${this.url}/${item.id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(item),
      })
        .then((response) => response.json())
        .then(() => {
          this.getTodos();
        });
    },
    editTodo(item) {
      this.temp = item;
      this.oldVal = item.content;
      this.isEdit = true;
    },
    delTodo(item) {
      if (!confirm(`確定要刪除待辦事項 - ${item.content}嗎？`)) {
        return;
      }
      fetch(`${this.url}/${item.id}`, {
        method: "DELETE",
      })
        .then((response) => response.json())
        .then(() => {
          this.getTodos();
        });
    },
    updateTodo(id, isCancel) {
      if (isCancel) {
        this.temp.content = this.oldVal;
        this.isEdit = !this.isEdit;
        return;
      }
      fetch(`${this.url}/${id}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(this.temp),
      })
        .then((response) => response.json())
        .then(() => {
          this.getTodos();
          this.isEdit = !this.isEdit;
        });
    },
    addTodo() {
      const data = {
        id: new Date().getUTCMilliseconds(),
        isDone: false,
        content: this.newTodo,
      };
      fetch(this.url, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(data),
      })
        .then((response) => response.json())
        .then(() => {
          this.newTodo = "";
          this.getTodos();
        });
    },
    getTodos() {
      fetch(this.url, {
        method: "GET",
      })
        .then((response) => response.json())
        .then((json) => (this.todos = json));
    },
  },
  mounted() {
    this.getTodos();
  },
};
</script>
