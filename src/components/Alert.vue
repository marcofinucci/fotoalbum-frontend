<script>
export default {
  name: "Alert",
  props: {
    title: String,
  },
};
</script>

<template>
  <div class="text-center alert alert-light">{{ title }}</div>
</template>
