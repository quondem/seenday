<template>
  <div
    class="block unload__card"
    :class="{ 'unload--success': status == 'green', 'unload--danger': status == 'red' }"
    @click="$emit('choose')"
  >
    <slot />
  </div>
</template>
<script>
export default {
  props: {
    status: {
      type: String,
      default: "green",
      required: false
    }
  },
  emits: ["choose"]
};
</script>
