<template>
  <div class="card p-3">
      <h4 class="text-primary">{{ data.title }}</h4>
      <p>{{ data.body }}</p>
      <p class="text-muted">{{ data.author }}</p>
  </div>

</template>

<script>
export default {
  props: {
    data: Object
  }
}
</script>