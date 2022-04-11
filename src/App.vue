<template>
  <div class="container">
    <img
      alt="Vue logo"
      class="logo"
      src="./assets/logo.svg"
      width="125"
      height="125"
    />
    <br />
    <todo-list></todo-list>
    <!-- เพื่อเรียกหน้าTodolist มาแสดงผล -->

    <!-- เรียกชื่อที่ตั้งไว้ที่หน้าTodoList -->

    <!-- <div class="wrapper">
      <HelloWorld msg="You did it!" />
     </div> -->
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import TheWelcome from "./components/TheWelcome.vue";
import TodoList from "./components/TodoList.vue"; //อ้างอิงชื่อเพื่อเรียกใช้งานตามชื่อที่ตั้งไว้ในคอมโพแนน

export default {
  name: "App",
  components: {
    TodoList,
  },
};
</script>


<style>
@import "./assets/base.css";

* {
  box-sizing: border-box;
}
.container {
  margin: 0 auto;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: auto;
  height: 75px;
}

a,
.green {
  text-decoration: none;
  color: hsla(160, 100%, 37%, 1);
  transition: 0.4s;
}

@media (hover: hover) {
  a:hover {
    background-color: hsla(160, 100%, 37%, 0.2);
  }
}

@media (min-width: 1024px) {
  body {
    display: flex;
    padding-top: 20px;
    /* place-items: center; */
  }

  #app {
    display: grid;
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
