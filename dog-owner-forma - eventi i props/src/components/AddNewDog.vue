<script setup>

const emit = defineEmits(['dog:add'])

const addNewDog = () => {
  emit('dog:add')
}
</script>

<template>
  <div class="row">
    <div class="col-12 mt-3 d-flex justify-content-end">
      <button @click="addNewDog" class="btn btn-success">+ Add Dog</button>
    </div>
  </div>
</template>