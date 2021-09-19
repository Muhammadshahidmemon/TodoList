<template>
    <section class="fixed w-full overflow-hidden inset-y-0 right-0 flex" v-show="value">  <transition
      mode="out-in"
        enter-active-class="ease-out duration-300"
        enter-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        enter-to-class="opacity-100 translate-y-0 sm:scale-100"
        leave-active-class="ease-in duration-200"
        leave-class="opacity-100 translate-y-0 sm:scale-100"
        leave-to-class="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
    >
  <div class="fixed w-full overflow-hidden min-h-screen bg-black bg-opacity-50 transition-opacity cursor-pointer" v-show="value" @click="onCancel">
  </div>
    </transition>
    <div class="flex-1">
    </div>


  <transition
    enter-active-class="transform transition ease-in-out duration-500 sm:duration-700"
    enter-class="translate-x-full"
    enter-to-class="translate-x-0"
    leave-active-class="transform transition ease-in-out duration-500 sm:duration-700"
    leave-class="translate-x-0"
    leave-to-class="translate-x-full">
    <div class="w-screen max-w-sm z-50 static"  v-show="value">
      <div class="h-full flex flex-col rounded-l bg-white shadow-xl overflow-y-auto">
        <div class="flex-1">
          <div @click="onCancel">
            <svg class="w-6 mt-6 ml-6 md:hidden" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M9.707 16.707a1 1 0 01-1.414 0l-6-6a1 1 0 010-1.414l6-6a1 1 0 011.414 1.414L5.414 9H17a1 1 0 110 2H5.414l4.293 4.293a1 1 0 010 1.414z" clip-rule="evenodd"/>
            </svg>
          </div>
          <div class="flex flex-col items-center justify-center min-h-screen mx-auto max-w-xs">
            <h1 class="text-3xl font-semibold">Write Your Task</h1>
            <input class="shadow mx-auto mt-4 appearance-none border rounded py-2 px-3 text-gray-700 leading-tight border-gray-500 focus:outline-none focus:shadow-outline w-full" type="text" v-model="task" placeholder="Type Task">
            <button class="shadow w-full mt-4 bg-purple-500 hover:bg-purple-400 focus:shadow-outline focus:outline-none text-white font-bold py-2 px-4 rounded" @click="AddTask">
            Add
          </button>
        </div>
        </div>
      </div>
    </div>
  </transition>
    </section>
</template>

<script>
const bodyScroll = require('body-scroll-toggle')
export default {
  props: {
    value: {
      type: Boolean,
      required: true
    }
  },
  watch: {
    value() {
      if (this.value) {
        bodyScroll.disable()
      } else {
        bodyScroll.enable()
      }
    }
  },
  data() {
    return {
      task: ''
    }
  },
  mounted() {
    console.log(this.fullView);
  },
  methods: {
    AddTask() {
      this.$emit('AddTask', this.task)
      this.task = ''
    },
    updateValue: function (value) {
      this.$emit('input', value)
    },
    onCancel () {
      this.$emit('onCancel')
    },
  }
}
</script>
