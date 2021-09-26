<script setup>
import { reactive, onUpdated } from 'vue'
import Owner from './components/Owner.vue';
import Dog from './components/Dog.vue';
import AddNewDog from './components/AddNewDog.vue';
// list all breeds - https://dog.ceo/api/breeds/list/all
// random from a breed - https://dog.ceo/api/breed/hound/images/random
// random from a sub-breed - https://dog.ceo/api/breed/hound/afghan/images/random

const getBreeds = async () => {
  const json = await fetch('https://dog.ceo/api/breeds/list/all').then(resp => resp.json());
  state.breeds = json.message;
}

getBreeds();

const state = reactive({
  breeds: {},
  owner: {
    name: '',
    surname: ''
  },
  dogs: []
})

const addNewDog = () => {
  const dog = reactive({
    name: '',
    breed: '',
    subBreed: '',
    gender: '',
    image: null
  });

  state.dogs.push(dog);
}

onUpdated(() => {
  console.log(state.dogs)
})

</script>

<template>
  <header
    class="d-flex flex-wrap justify-content-center py-3 mb-4 border-bottom bg-success text-white"
  >
    <span class="fs-4">Dog Owner Form</span>
  </header>

  <main class="flex-shrink-0">
    <div class="container">
      <div class="row">
        <div class="col-6 m-auto">
          <div class="row">
            <Owner :owner="state.owner" />
          </div>

          <div class="row">
            <div class="col">
              <h3 class="mb-3">Dogs</h3>

              <div class="dogs">
                <Dog v-for="dog in state.dogs" :dog="dog" :breeds="state.breeds" />
              </div>

              <AddNewDog @dog:add="addNewDog" />

              <div class="col-12 mt-3 d-flex justify-content-end">
                <button class="btn btn-secondary" disabled>Submit Form</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>

  <footer class="footer mt-4 py-3 bg-light">
    <div class="container">
      <footer class="text-center">Dog Owner Form - {{ new Date(Date.now()).getFullYear() }}</footer>
    </div>
  </footer>
</template>

<style>
.dog-image {
  position: absolute;
  height: 100px;
  width: 100px;
  right: -15px;
  top: -15px;
  border-radius: 50%;
  border: 2px solid;
}
</style>