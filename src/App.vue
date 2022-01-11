<template>
  <div>
    <header>
      <h1> 11th January, 2022 vue-ts blank test. </h1>
    </header>
    <main>
      <TodoInput :item="todoItem" @input="newTodoItem" @add="addTodoItem"></TodoInput>
      <ul>
        <TodoListItems v-for="(todoItem, index) in todoItems" :key="index" :todoItem="todoItem" :index="index" @toggle="toggleTodoItem" @remove="removeTodoItem"></TodoListItems>
      </ul>
    </main>
  </div>
</template>

<script lang="ts">
    import Vue from 'vue'
    import TodoInput from "@/components/TodoInput.vue";
    import TodoListItems from "@/components/TodoListItems.vue";

    export interface Todo {
      title: string,
      done: boolean
    }

    const STORAGE_KEY = "11-jan-2022"
    const storage = {
      save(value: Todo[]) { // 여기를 Todo로 해놨어서 error. Todo[]였어야지
        const parsed = JSON.stringify(value)
        localStorage.setItem(STORAGE_KEY, parsed)
      },
      fetch(): Todo[] {
        const value = localStorage.getItem(STORAGE_KEY) || "[]";
        const result = JSON.parse(value)
        return result
      }
    }

    export default Vue.extend ({
      components: {TodoListItems, TodoInput},
      data() {
        return {
          todoItem: "props data.",
          todoItems: [] as Todo[]
        }
      },
      methods: {
        newTodoItem(value: any) {
          this.todoItem = value;
        },
        addTodoItem() {
          const value = this.todoItem
          const values = {
            title: value,
            done: false
          }
          this.todoItems.push(values)
          storage.save(this.todoItems); // 여기서 왜 error가 나지???? 뭐지? => save의 argument type을 Todo[] 아닌 Todo로 줘서.
          this.initTodoItem()
        },
        fetchTodoItem() {
          this.todoItems = storage.fetch().sort((a,b) => { // 여기서 또 헤매네. this.todoItems = 담았어야지!!!!!!
            if (a.title > b.title) {
              return 1;
            }
            if (a.title < b.title) {
              return -1;
            }
            return 0
          })
          storage.save(this.todoItems)
        },
        initTodoItem() {
          this.todoItem = ""
        },
        toggleTodoItem(value: Todo, index: number) {
          this.todoItems.splice(index, 1, { // 여길 잘 못쓰네 음 => 이것도 역시 type. Todo로 줘야 이게 정상적으로 먹네.
            ...value,
            done: !value.done
          })
          storage.save(this.todoItems) // save 해줘야함
        },
        removeTodoItem(index: number) {
          this.todoItems.splice(index, 1)
          storage.save(this.todoItems)
        }
      },
      created() {
        this.fetchTodoItem();
      }

    })
</script>

<style scoped>

</style>