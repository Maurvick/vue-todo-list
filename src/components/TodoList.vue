<template>
  <div class="container">
    <form>
      <h1>Todo list</h1>
      <input
        v-model="inputValue"
        class="todo-input"
        placeholder="Write something and press enter..."
      />
      <button hidden @click="addItemToList()">add todo</button>
      <button hidden @click="clearLocalStorage()">Clear todo list</button>
      <ul>
        <li
          v-bind:key="item"
          v-for="item in list"
          :class="{ completed: item.isComplete }"
        >
          <input
            type="checkbox"
            v-model="item.isComplete"
            @click="handleTodoCheck(item.isComplete)"
          />
          {{ item.name }}
          <span @click="deleteItemById(item.id)" class="cross"></span>
        </li>
      </ul>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      inputValue: "",
    };
  },
  mounted() {
    if (localStorage.getItem("vue") !== null) {
      try {
        this.list = JSON.parse(localStorage.getItem("vue"));
      } catch (e) {
        console.error("Failed to parse localStorage data:", e);
      }
    }
  },
  methods: {
    addItemToList() {
      if (this.inputValue != null && this.inputValue != "") {
        this.list.push({
          id: this.list.length + 1,
          name: this.inputValue,
          isComplete: false,
        });
        this.clearInput();
      }
    },
    deleteItemById(id) {
      this.list = this.list.filter((item) => item.id !== id);
    },
    handleTodoCheck(todo) {
      return !todo.isComplete;
    },
    clearInput() {
      this.inputValue = "";
    },
    clearLocalStorage() {
      localStorage.clear();
    },
  },
  watch: {
    list: {
      handler(newValue) {
        try {
          localStorage.setItem("vue", JSON.stringify(newValue));
          console.log(newValue);
        } catch (e) {
          console.error("Failed to stringify data:", e);
        }
      },
      deep: true,
    },
  },
  computed: {},
};
</script>

<style scoped>
.todo-input {
  padding: 10px;
  display: block;
  width: 95%;
  border: 1px solid black;
  border-radius: 5px;
}
form {
  background-color: #fff;
  max-width: 500px;
  margin: 50px auto;
  padding: 30px 20px;
  box-shadow: 2px 5px 10px rgba(0, 0, 0, 0.5);
}
ul li {
  list-style-type: none;
  position: relative;
  font-size: 24px;
}
ul {
  padding: 0;
  margin: 5;
}

li {
  border: 1px solid black;
  word-break: break-all;
  padding: 15px 15px 15px 60px;
  display: block;
  line-height: 1.2;
  margin: 5px;

  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cross::before {
  content: "x"; /* Unicode character for multiplication sign */
  font-size: 20px;
  color: black; /* Optional: Change color */
}
.cross:hover {
  cursor: pointer;
}
.completed {
  text-decoration: line-through;
}
</style>
