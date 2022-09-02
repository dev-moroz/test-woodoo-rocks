<template>

  <div class="card" style="width: 18rem;">
    <div class="card-body">
      <h4 class="card-title text-primary">{{ data.title }}</h4>
      <p class="card-text">{{ data.body }}</p>
      <p class="card-text text-muted">{{ data.author }}</p>
    </div>
  </div>

</template>

<script>
export default {

  props: {
    data: Object
  }
}
</script>
