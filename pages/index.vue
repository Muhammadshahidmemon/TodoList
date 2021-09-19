<template>
  <div>
    <notification-wrapper />
    <Modal
      title="Are you sure to delete this task"
      v-model="modalStates.deleteTask"
      modalTypeStyles="bg-red-600 hover:bg-red-500 focus:shadow-outline-red"
      @onOk="onDeleteTask()"
      @onCancel="modalStates.deleteTask = false"
      primaryButtonText="Delete"
      cancelButtonText="Cancel"
    />
    <div class="w-full md:min-h-screen bg-gray-100">
      <div class="md:container md:mx-auto py-6 px-4 md:p-10">
        <div class="flex mb-6">
          <div class="w-1/12">
          <div class="mt-3">
            <!-- <ProgressRing :percentage="percentage" /> -->
          </div>
          </div>
          <div class="w-10/12">
            <h1 class="text-3xl font-semibold">My Task</h1>
            <h3 class="text-xs ml-1 mt-1 mb-6 font-semibold text-gray-500">{{this.completedTask}} of {{taskList.length}} tasks</h3>
            <hr>
          </div>
        </div>
        <div class="flex h-12" v-for="(row, key) in taskList" :key="key" @mouseover="row.hover = true" @mouseleave="row.hover = false">
          <div class="w-1/12">
            <input
              type="checkbox"
              v-if="!row.isCompleted"
              v-model="row.isCompleted"
              @input="completedTask += 1"
              class="form-checkbox cursor-pointer border-gray-500 h-4 w-4 text-green-600 transition duration-150 ease-in-out"
            />
          </div>
          <div class="w-10/12">
          <RoughNotation
            :is-show="row.isCompleted"
            type="strike-through"
          >
            <h2 class="text-base text-gray-800 font-semibold">{{ row.name }}</h2>
          </RoughNotation>
          </div>
          <div class="w-1/12">
            <button
            v-if="row.hover"
                type="button"
                aria-label="Deletar"
                @click="modalStates.deleteTask = true;deleteTaskId = key;isCompletedDeleteTask = row.isCompleted"
                class="-ml-px relative inline-flex items-center pl-2 py-2 rounded-r-md border border-gray-300 bg-white text-sm leading-5 font-medium text-gray-500 hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-500 transition ease-in-out duration-150"
              >
                <svg
                  viewBox="0 0 20 20"
                  fill="currentColor"
                  class="mr-2 h-4 w-4"
                >
                  <path
                    fill-rule="evenodd"
                    d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                    clip-rule="evenodd"
                  ></path>
                </svg></button
            >
          </div>
        </div>
        <button @click="slideoverStates.view = true" class="p-2 md:mr-16 float-right mt-6 text-center focus:outline-none hover:bg-green-500 bg-green-600 rounded">
          <svg class="w-6 text-white" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
            <path fill-rule="evenodd" d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z" clip-rule="evenodd"/>
          </svg> 
        </button>
      </div>
    </div>
    <Slideover v-show="slideoverStates.view" v-model="slideoverStates.view" @onCancel="slideoverStates.view = false" @AddTask="AddTask" />
  </div>
</template>

<script>
import ProgressRing from '@/components/ProgressRing' 
export default {
  components: {
    ProgressRing
  },
  data() {
    return {
      // percentage: 0,
      modalStates: {
        deleteTask: false
      },
      taskList: [
        
      ],
      slideoverStates: {
        view: false
      },
      completedTask: 0,
      deleteTaskId: null,
      isCompletedDeleteTask: false
    }
  },
  methods: {
    onDeleteTask() {
      this.modalStates.deleteTask = false
      if (this.isCompletedDeleteTask) {
        this.completedTask -= 1
      }
      this.taskList.splice(this.deleteTaskId, 1)
      this.$notification({
        text: 'Task Deleted',
        color: 'red',
      });
      this.deleteTaskId = null
      // this.percentage = (this.taskList.length / this.completedTask) * 100
    },
    AddTask(task) {
      console.log(task);
      this.taskList.push({name: task, isCompleted: false, hover: false})
      this.slideoverStates.view = false
      this.$notification({
        text: 'Task Added',
        color: 'green',
      });
      // this.percentage = (this.taskList.length / this.completedTask) * 100
    }
  }
}
</script>