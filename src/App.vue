<script>
import { computed, reactive, toRefs } from '@vue/reactivity'

export default {
  setup() {
    const state = reactive({
      newTaskLabel: '',
      newTaskType: '',
      newTaskEstimate: 0,
      taskList: [],
      needTaskList: computed(() => {
        return state.taskList.filter(item => item.type === 'Need')
      }),
      wantTaskList: computed(() => {
        return state.taskList.filter(item => item.type === 'Want')
      })
    })

    const addNewTask = () => {
      state.taskList.push({
        label: state.newTaskLabel,
        type: state.newTaskType,
        estimate: state.newTaskEstimate
      })
    }

    return {
      ...toRefs(state),
      addNewTask
    }
  }
}
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <form @submit.prevent class="new-task-form">
    <label for="task-label">Task Label</label>
    <input type="text" id="task-label" v-model="newTaskLabel" />

    <label for="task-type">Task Type:</label>
    <input
      list="task-type-list"
      type="text"
      id="task-type"
      v-model="newTaskType"
    />
    <datalist id="task-type-list">
      <option value="Need"></option>
      <option value="Want"></option>
    </datalist>

    <label for="task-estimate">Estimate:</label>
    <input type="number" v-model="newTaskEstimate" />

    <button @click="addNewTask">Add</button>
  </form>

  <h2>Task List</h2>

  <div class="task-list-wrapper">
    <section>
      <h3>Need</h3>
      <ul>
        <li v-for="item in needTaskList" :key="item.label">
          {{ item.label }} ({{ item.type }}) — {{ item.estimate }} min
        </li>
      </ul>
    </section>
    <section>
      <h3>Want</h3>
      <ul>
        <li v-for="item in wantTaskList" :key="item.label">
          {{ item.label }} ({{ item.type }}) — {{ item.estimate }} min
        </li>
      </ul>
    </section>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.new-task-form {
  display: flex;
  flex-direction: column;
  max-width: 640px;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
}

.task-list-wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
}

input {
  margin-bottom: 1rem;
}
</style>
