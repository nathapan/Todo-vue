<template>
  <div>
    <div class="todo-item">
      <input type="checkbox" v-model="myTodo.completed" />
      <label :class="myTodo.completed == true ? 'lineThorough' : 'title-label'">
        {{ myTodo.title }}
      </label>
    </div>
  </div>
  <div class="CursorPointer" @click="removeItem()">&times;</div>
</template>
<!-- <div class="CursorPointer" @click="$emit('remove', index)">&times;</div>  เขียนแบบไม่ต้องไปเรียกใช้ methods-->

<script>
// import TodoItem from "./TodoItem.vue";
export default {
  // components: { TodoItem },
  props: ["myTodo", "index"],
  data() {
    return {
      checked: false,
      test: [],
    };
  },
  methods: {
    removeItem() {
      this.$emit("remove", this.index);
    },
  },
};
</script>

<style>
.lineThorough {
  text-decoration: line-through;
  color: green;
  padding-left: 11px;
}

.title-label {
  padding-left: 10px;
}
.CursorPointer {
  cursor: Pointer;
  margin-left: 400px;
  margin-top: -30px;
  color: red;
}
</style>