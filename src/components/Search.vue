<template>
  <div class="input-search my-3 m-auto d-flex">
      <label for="search">Search: </label>
      <input 
        id="search"
        class="w-100"
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
      />
    </div>
</template>

<script>
export default{
  name: 'SearchComponent',
  props: {
    modelValue: {
      type: String
    }
  }
}

</script>