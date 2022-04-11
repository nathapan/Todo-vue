<template>
  <div>
    <input
      type="text"
      class="todo-input"
      placeholder="What needs to be done"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />

    <!-- <transition-group
      name="fade"
      enter-active-class="animated fadeInUp"
      leave-active-class="animated fadeOutDown"
    > -->

    <todo-item
      v-for="(todo, index) in todosFilterd"
      :key="todo.id"
      :index="index"
      @remove="removeTodo($event)"
      class="todo-item"
      :myTodo="todo"
    >
    </todo-item>

    <!-- <div class="todo-item-left">
        <input type="checkbox" v-model="todo.completed" />
        <div
          v-if="!todo.editing"
          @dblclick="editTodo(todo)"
          class="todo-item-label"
          :class="{ completed: todo.completed }"
        >
          {{ todo.title }}
        </div>
        <input
          v-else
          class="todo-otem-edit"
          type="text"
          v-model="todo.title"
          @blur="doneEdit(todo)"
          @keyup.enter="doneEdit(todo)"
          @keyup.esc="cancelEdit(todo)"
          v-focus
        />
      </div>
      <div class="remove-item" @click="removeTodo(index)">&times;</div> -->
    <!-- </transition-group> -->

    <div class="extra-container">
      <div>
        <label
          ><input
            type="checkbox"
            :checked="!anyRemaining"
            @change="checkAllTodos"
          />
          checkAll</label
        >
        <!-- ในส่วนปุ่ม checkAll เมื่อคลิกแล้วให้เปลี่ยนค่าในการแสดงผล items left -->
      </div>

      <div>{{ remaining }}items left</div>
      <!-- เมื่อกดเช็คแล้ว ให้แสดงผลว่าเหลื่ออะไรบ้าที่ต้องทำ  เข้ากับloop checkAll -->
    </div>
    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">
          All
        </button>
        <button
          :class="{ active: filter == 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>
        <button
          :class="{ active: filter == 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>
      <!-- class extra container  ปุ่มกด all, Active and completed -->

      <div>
        <transition name="fade">
          <button v-if="showClearCompletdButton" @click="clearCompleted">
            clear Completed
          </button>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import TodoItem from "./TodoItem.vue";

// การอ้างชื่อไฟล์อีกไฟล์มาแสดงอีกหน้าหนึ่ง  **ชื่อไฟล์ต้องตรงกับที่สร้างไฟล์ไว้**
export default {
  components: { "todo-item": TodoItem },
  data() {
    return {
      newTodo: "",
      idForTodo: "3",
      beforeEditCache: "",
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Finish Vue Screencast",
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: "Take over word",
          completed: false,
          editing: false,
        },
      ],

      todosbackup: [
        {
          id: 1,
          title: "Finish Vue Screencast",
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: "Take over word",
          completed: false,
          editing: false,
        },
      ],
    };
  },
  computed: {
    mapTodo() {
      return this.todos.map((todo) => todo.title);
    },
    filterTodo() {
      return this.todos.filter((todo) => {
        return todo.id > 1;
      });
    },
    remaining() {
      return this.todos.filter((todo) => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining != 0;
    },
    todosFilterd() {
      //เช็คด้วย if
      // console.log("filter", this.filter);
      if (this.filter == "active") {
        return this.todos.filter((todo) => !todo.completed);
      } else if (this.filter == "completed") {
        return this.todos.filter((todo) => todo.completed);
      }
      if (this.todos.length == 0) {
        this.todos = this.todosbackup;
      }
      return this.todos;
    },
    showClearCompletdButton() {
      return this.todos.filter((todo) => todo.completed).length > 0;
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return; //เมื่อเรากด enter ให้จำกัดอาร์เรย์ เพื่อไม่ได้เกิดทำงานซ้ำซาก
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false,
      });

      this.newTodo = "";
      this.idForTodo++;

      //alert('adding') //alert เมื่อกรอกข้อความลงบน textBox
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },
    doneEdit(todo) {
      if (todo.title.trim() == "") {
        todo.title = this.beforeEditCache;
      }
      todo.editing = false;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    },
    checkAllTodos(event) {
      this.todos.forEach((todo) => (todo.completed = event.target.checked));
      // for (let i = 0; i < this.todos.length; i++) {
      //   this.todos[i].completed = event.target.checked;
      // }
    },
    clearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.completed);
    },
  },
};
</script>



<style lang="scss">
@import url("https://cdnjs.coudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input {
  padding: 10px 18px;
  width: 412px;
  font-size: 18px;
  margin-bottom: 16px;
  &:focus {
    outline: 0;
  }
}
.todo-item {
  margin-bottom: 12px;
  //display: flex;
  align-items: center;
  justify-content: space-between;
  //   animation: bounce;
  //   animation-duration: 4s;
}
.remove-item {
  cursor: pointer;
  margin-left: 14px;
  &:hover {
    color: black;
  }
}
.todo-item-left {
  display: flex;
  align-items: center;
}
.todo-item-label {
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}
// .todo-item-animate{
//      animation: bounce;
//      animation-duration: 2s;

// }
.todo-item-edit {
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: "Avenir", Helvetica, Arial, sans-serif;

  &:focus {
    outline: none;
  }
}
.completed {
  text-decoration: line-through;
  color: grey;
}
.extra-container {
  //css ในส่วน checkbox ด้านล่าง
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid rgb(24, 20, 20);
  padding-top: 14px;
  margin-bottom: 14px;
}
button {
  font-size: 14px;
  background-color: white;
  appearance: none;

  &:hover {
    background: lightgreen;
  }
  &:focus {
    outline: none;
  }
}
.active {
  background: lightgreen;
}

//css transitions
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

// :root {
//   --animate-duration: 10s;
// }

// /* Only this element will take half the time to finish */
// .my-element {
//   --animate-duration: s;
// }
</style>
