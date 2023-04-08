<script>
import { computed } from "@vue/reactivity";
import List from "./components/List.vue";
import { ref, reactive, onMounted, toRefs } from "vue";
export default {
  components: { List },
  setup() {
    const todo = ref('');
    const todos = reactive({
      list: [],
    });

    onMounted(() => {
      if (localStorage.getItem('todos') == undefined) {
        localStorage.setItem('todos', JSON.stringify([]));
      }
      todos.list = JSON.parse(localStorage.getItem('todos'))
    });

    const totalTODO = computed(() => {
      if (todos.list) {
        return todos.list.length
      } else {
        return 0
      }
    })

    const add = () => {
      todos.list.unshift(
        {
          activity: todo.value,
          isDone: false
        }
      );
      todo.value = "";
      saveToLocalStorage();
    }

    const deleteTODO = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index != todoIndex) {
          return item
        }
      });
      saveToLocalStorage();
    }

    const doneTodo = (todoIndex) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == todoIndex) {
          if (item.isDone) {
            item.isDone = false
          } else {
            item.isDone = true
          }
        }
        return item
      });
      saveToLocalStorage();
    }

    const saveToLocalStorage = () => {
      localStorage.setItem('todos', JSON.stringify(todos.list));
    }

    const clearStorage = () => {
      localStorage.clear();
      todos.list = [];
    }


    return { todo, ...toRefs(todos), totalTODO, add, deleteTODO, doneTodo, clearStorage }
  },
}
</script>

<template>
  <div class="container" style="margin-top: 20px; width: 100%;">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title"> Simple To Do App</h5>
        <p>this app created using Vue JS and save current data to local storage</p>
        <div class="row">
          <div class="col-10">
            <input v-model="todo" @keyup.enter="add" type="text" class="form-control">
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">ADD</button>
          </div>
        </div>
        <List :todos="list" @deleteTODO="deleteTODO" @doneTodo="doneTodo" />
        <br>
        <div class="row">
          <div class="col-8">
            <small>Total To Do : {{ totalTODO }}</small>
          </div>
          <div class="col-4">
            <button class="btn btn-danger" @click="clearStorage">Clear All Data</button>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

