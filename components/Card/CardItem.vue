<template>
  <div
    :class="[
      'item-task d-flex align-items-start border-bottom pt-3 pb-4',
      isGrid ? 'col-12 col-md-6 col-lg-4' : 'col-12',
    ]"
  >
    <input
      id="tasks"
      v-model="localTask.isDone"
      type="checkbox"
      name="status"
      class="me-2 mt-2"
      :checked="localTask.isDone"
      @change="updateTaskStatus"
    />

    <div
      :class="[
        'd-flex flex-column',
        localTask.isDone ? 'text-decoration-line-through fst-italic' : '',
      ]"
    >
      <div class="title-task mb-1">
        {{ localTask.title }}
      </div>
      <div class="description-task small text-muted">
        {{ localTask.description }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
      default() {
        return {}
      },
    },
    isGrid: {
      type: Boolean,
      required: true,
      default: false,
    },
  },
  computed: {
    localTask: {
      get() {
        return this.task
      },
      set(newValue) {
        this.$emit('update:task', newValue)
      },
    },
  },
  methods: {
    updateTaskStatus() {
      this.$emit('updateTaskStatus', this.localTask)
    },
  },
}
</script>
