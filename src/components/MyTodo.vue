<template>
  <div class="container">
    <div class="row">
      <div class="col-7 mx-auto">
        <h1 class="text-left">My Tasks ({{ toDo.length }})</h1>
        <div class="input-group mb-2">
          <input
            class="form-control new-to-do"
            :class="{'fieldEmpty': fillTheField}"
            type="text"
            placeholder="New task here"
            ref="newTaskField"
            v-model="newtask"
          >
          <button class="btn-add-task input-group-append" @click="addTask()">
            <span class="input-group-text">+ Add</span>
          </button>
        </div>
      </div>
    </div> 
    <div class="row" v-if="toDo.length">
      <div class="checkboxes-to-do mx-auto mb-2">
        <div class="custom-control custom-checkbox text-right mr-3">
          <input
            type="checkbox"
            class="custom-control-input"
            id="checkAllOrUnckeckAll"
            @click="checkAllOrUnckeckToDo()"
          >
          <label class="custom-control-label" for="checkAllOrUnckeckAll"> Check all/Unckeck all</label>
        </div>
        <div>
          <button
            class="btn btn-sm btn-outline-danger"
            @click="toDo=[]"
          >
            Remove all
          </button>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-7 mx-auto">
        <div class="list-tasks mt-1">
          <TransitionGroup
            name="list"
          >
            <div
              class="alert alert-primary item-to-do"
              :class="{'alert-success': toDoItem.finished}"
              v-for="(toDoItem, i) in toDo" :key="i"
              @click="toDoItem.finished = !toDoItem.finished"
            >
              <p
                class="title-to-do"
                :class="{'checked': toDoItem.finished}">
                {{ toDoItem.text }}
              </p>
              <button
                @click.stop="removeToDo(i)"
                class="btn-remove-to-do btn btn-sm btn-danger"
              >
                X
              </button>
            </div>
          </TransitionGroup>
        </div>
        <div class="list-tasks" v-if="!toDo.length">
          <h5>Add a task above</h5>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { globalCookiesConfig, useCookies } from "vue3-cookies";
const { cookies } = useCookies();

globalCookiesConfig({
  expireTimes: "30d",
  path: "/",
  domain: "",
  secure: true,
  sameSite: "None",
});

export default {
  name: 'MyTodo',
  data() {
    return {
      cookies: useCookies(),
      fillTheField: false,
      newtask: '',
      checkOrUnckecktAll: false,
      toDo: []
    }
  },
  mounted() {
    console.log(cookies.get())
    // let my_cookie_value = this.cookies.get("myCoookie");
    // console.log(my_cookie_value);
    // this.cookies.set("myCoookie", "abcdefg");
    // let my_cookie_value = this.cookies.get("myCoookie");
  },
  methods: {
    addTask() {
      
      if (!this.newtask) {
        this.fillTheField = true
        this.$refs.newTaskField.focus()
        return false
      }

      this.fillTheField = false
      this.toDo.push({
        text: this.newtask,
        finished: false
      })
      this.newtask = ''
    },
    checkAllOrUnckeckToDo() {
      this.checkOrUnckecktAll = !this.checkOrUnckecktAll

      if (this.toDo.length > 0) {
        if (this.checkOrUnckecktAll) {
          this.toDo.forEach((item) => {
            item.finished = true
          })
        } else {
          this.toDo.forEach((item) => {
            item.finished = false
          })
        }
      }
    },
    removeToDo(index) {
      this.toDo.splice(index, 1)
    }
  }
}
</script>

<!-- botar depois como sass -->
<style scoped>
.title-to-do{
  display: inline-block;
  user-select: none;
  margin: 0;
  padding: 0;
  animation-duration: 0.4s;
}
.new-to-do {
  border-radius: 50px 0 0 50px;
}
.new-to-do:focus {
  box-shadow: 0 0 0 1px rgba(0,123,255,.25);
}
.fieldEmpty,
.fieldEmpty:focus {
  border-color: #ff0000;
  box-shadow: 0 0 0 0.2px #ff0000;
}
.checkboxes-to-do {
  display: flex;
  align-items: center;
  user-select: none;
}
.btn-remove-to-do {
  float: right;
  z-index: 9999;
}
.checked {
  text-decoration: line-through;
  font-weight: bold;
}
.item-to-do:hover {
  cursor: pointer;
}
.btn-add-task {
  border-radius: 0 50px 50px 0;
  background: #13d513;
  user-select:none;
}
.btn-add-task:focus {
  outline: none;
  background-color: #38b438;
}
.input-group-append {
  border: none;
}
.input-group-text {
  border: none;
  background-color: transparent;
  color: #fff;
}
.list-enter-active,
.list-leave-active {
  transition: all 0.4s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(20px);
}
</style>
