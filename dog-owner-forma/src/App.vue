<script setup>
import { reactive, computed, onUpdated } from 'vue'
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

const listOfBreeds = computed(() => Object.keys(state.breeds));

const hasSubBreed = (breed) => state.breeds[breed]?.length > 0;

const getSubBreed = (breed) => state.breeds[breed];

const dogNameExists = (dogName) => {
  return dogName !== ''
};

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

const getBreedImage = async (dog) => {
  if (!hasSubBreed(dog.breed)) {
    const json = await fetch(`https://dog.ceo/api/breed/${dog.breed}/images/random`).then(resp => resp.json());
    dog.image = json.message;
  }
}

const getSubBreedImage = async (dog) => {
  const json = await fetch(`https://dog.ceo/api/breed/${dog.breed}/${dog.subBreed}/images/random`).then(resp => resp.json());
  dog.image = json.message;
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
            <h3>Owner {{ state.owner.name }} {{ state.owner.surname }}</h3>
            <div class="row mb-3">
              <div class="col-6">
                <label for="name" class="form-label">Name</label>
                <input v-model="state.owner.name" type="text" class="form-control" id="name" />
              </div>
              <div class="col-6">
                <label for="surname" class="form-label">Surname</label>
                <input v-model="state.owner.surname" type="text" class="form-control" id="surname" />
              </div>
            </div>
          </div>

          <div class="row">
            <div class="col">
              <h3 class="mb-3">Dogs</h3>

              <div class="dogs">
                <div v-for="dog in state.dogs" class="card mb-3">
                  <img
                    v-if="dog.image"
                    class="dog-image"
                    :class="{ 'border-male': dog.gender === 'male', 'border-female': dog.gender === 'female' }"
                    :src="dog.image"
                    alt
                  />
                  <h5
                    class="card-header"
                    :class="{ 'bg-male': dog.gender === 'male', 'bg-female': dog.gender === 'female' }"
                  >{{ dogNameExists(dog.name) ? dog.name : 'Dog Name' }}</h5>
                  <div class="card-body">
                    <div class="row">
                      <div class="col-12 mb-3">
                        <label for="name" class="form-label">Name</label>
                        <input v-model="dog.name" type="text" class="form-control" id="name" />
                      </div>
                      <div class="col-12 mb-3">
                        <div class="row">
                          <div class="col-6">
                            <label for="name" class="form-label">Breed</label>
                            <select
                              v-model="dog.breed"
                              @change="getBreedImage(dog)"
                              class="form-select"
                              name="breed"
                              id="breed"
                            >
                              <option v-for="breed in listOfBreeds" :value="breed">{{ breed }}</option>
                            </select>
                          </div>
                          <div v-if="hasSubBreed(dog.breed)" class="col-6">
                            <label for="name" class="form-label">Sub-Breed</label>
                            <select
                              v-model="dog.subBreed"
                              @change="getSubBreedImage(dog)"
                              class="form-select"
                              name="breed"
                              id="breed"
                            >
                              <option
                                v-for="subBreed in getSubBreed(dog.breed)"
                                :value="subBreed"
                              >{{ subBreed }}</option>
                            </select>
                          </div>
                        </div>
                      </div>
                      <div class="col-12">
                        <label for="gender" class="form-label">Gender</label>
                        <div>
                          <div class="form-check form-check-inline">
                            <input
                              class="form-check-input"
                              type="radio"
                              name="gender-group"
                              id="inlineRadio1"
                              value="male"
                              v-model="dog.gender"
                            />
                            <label class="form-check-label" for="inlineRadio1">Male</label>
                          </div>
                          <div class="form-check form-check-inline">
                            <input
                              class="form-check-input"
                              type="radio"
                              name="gender-group"
                              id="inlineRadio2"
                              value="female"
                              v-model="dog.gender"
                            />
                            <label class="form-check-label" for="inlineRadio2">Female</label>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="row">
                <div class="col-12 mt-3 d-flex justify-content-end">
                  <button @click="addNewDog" class="btn btn-success">+ Add Dog</button>
                </div>
              </div>

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