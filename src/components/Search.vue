<template>
  <div class="input-search my-3 m-auto d-flex">
      <label for="search">Search: </label>
      <input 
        id="search"
        class="w-100"
        :value="letter" 
        @input="$emit('input', $event.target.value)"
      />
    </div>
</template>

<script>
export default{
  name: 'SearchComponent',
  props: {
    letter: {
      type: String
    }
  }
}

</script>