<template>
  <div class="fixed bottom-0 z-50 inset-x-0 px-4 pb-4 sm:inset-0 sm:flex sm:items-center sm:justify-center" v-show="value">
    <div class="fixed inset-0 transition-opacity" @click="onCancel">
      <div class="absolute inset-0 bg-gray-500 opacity-75"></div>
    </div>

    <div class="bg-white overflow-hidden rounded-lg shadow-xl transform transition-all sm:w-full" role="dialog" aria-modal="true" aria-labelledby="modal-headline" :class="width">
      <div class="bg-white px-4 pt-5 pb-4 sm:p-6">
        <div>
          <div class="mt-3 sm:mt-0 sm:ml-4 sm:text-left">
            <h3 class="text-lg leading-6 font-medium text-gray-900" id="modal-headline">
              {{ title }}
            </h3>
            <div class="mt-2" v-if="description">
              <p class="text-sm leading-5 text-gray-500" v-html="description">
              </p>
            </div>
            <div class="mt-2">
              <slot></slot>
            </div>
          </div>
        </div>
      </div>
      <div class="bg-gray-50 px-4 py-3 sm:px-6 sm:flex sm:flex-row-reverse">
        <span class="flex w-full rounded-md shadow-sm sm:ml-3 sm:w-auto">
          <button
            type="button"
            class="cursor-pointer inline-flex justify-center w-full rounded-md border border-transparent px-4 py-2 text-base leading-6 font-medium text-white shadow-sm focus:outline-none focus:border-red-700 transition ease-in-out duration-150 sm:text-sm sm:leading-5"
            :class="[modalTypeStyles, {'opacity-50': isLoading}]"
            @click='onOk'
          >
          <LoadingSpinner v-if="isLoading" /> {{ primaryButtonText }}
          </button>
        </span>
        <span class="mt-3 flex w-full rounded-md shadow-sm sm:mt-0 sm:w-auto">
          <button @click="onCancel" type="button" class="cursor-pointer inline-flex justify-center w-full rounded-md border border-gray-300 px-4 py-2 bg-white text-base leading-6 font-medium text-gray-700 shadow-sm hover:text-gray-500 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue transition ease-in-out duration-150 sm:text-sm sm:leading-5">
            {{ cancelButtonText }}
          </button>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Boolean
    },
    title: {
      type: String,
    },
    description: {
      type: String,
    },
    primaryButtonText: {
      type: String,
      default: 'Yes'
    },
    cancelButtonText: {
      type: String,
      default: 'No'
    },
    modalTypeStyles: {
      type: String,
    },
    width: {
      type: String,
      default: 'sm:max-w-lg'
    },
    isLoading: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    updateValue: function (value) {
      this.$emit('input', value)
    },
    onOk () {
      this.$emit('onOk')
    },
    onCancel () {
      this.$emit('onCancel')
    },
  }
}
</script>
