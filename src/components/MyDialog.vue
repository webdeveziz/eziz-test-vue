<template>
  <div class="dialog" v-if="show" @click="hideDialog">
    <div class="dialog__content" @click.stop>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'my-dialog',
  props: {
    show: {
      type: Boolean,
      default() {
        return false
      }
    }
  },
  methods: {
    hideDialog() {
      this.$emit('update:show', false)
    }
  }
}
</script>

<style >
.dialog {
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.dialog__content {
  margin: auto;
  background: #fff;
  border-radius: 12px;
  min-width: 300px;
  min-height: 50px;
  padding: 20px;
}
</style>