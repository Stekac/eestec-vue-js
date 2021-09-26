<script setup>
defineProps(['owner'])

</script>

<template>
  <h3>Owner {{ owner.name }} {{ owner.surname }}</h3>
  <div class="row mb-3">
    <div class="col-6">
      <label for="name" class="form-label">Name</label>
      <input v-model="owner.name" type="text" class="form-control" id="name" />
    </div>
    <div class="col-6">
      <label for="surname" class="form-label">Surname</label>
      <input v-model="owner.surname" type="text" class="form-control" id="surname" />
    </div>
  </div>
</template>