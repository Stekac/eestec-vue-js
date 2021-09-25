<script setup>
import { reactive, computed, ref } from 'vue'
// list all breeds - https://dog.ceo/api/breeds/list/all
// random from a breed - https://dog.ceo/api/breed/hound/images/random
// random from a sub-breed - https://dog.ceo/api/breed/hound/afghan/images/random

let breeds = reactive({ list: [] });

const getBreeds = async () => {
  const json = await fetch('https://dog.ceo/api/breeds/list/all').then(resp => resp.json());
  console.log(Object.keys(json.message))
  breeds.list = Object.keys(json.message);
}

getBreeds();



const owner = reactive({
  name: '',
  surname: ''
})

const dog = reactive({
  name: '',
  breed: '',
  subBreed: '',
  gender: ''
});

const dogs = reactive({
  list: [dog]
})

const dogNameExists = computed(() => {
  return dog.name !== ''
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
          </div>

          <div class="row">
            <div class="col">
              <h3 class="mb-3">Dogs</h3>

              <div class="dogs">
                <div v-for="dog in dogs.list" class="card mb-3">
                  <img class="dog-image border-female" src="https://via.placeholder.com/150" alt />
                  <h5 class="card-header bg-female">{{ dogNameExists ? dog.name : 'Dog Name' }}</h5>
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
                            <select v-model="dog.breed" class="form-select" name="breed" id="breed">
                              <option v-for="breed in breeds.list" :value="breed">{{ breed }}</option>
                            </select>
                          </div>
                          <div class="col-6">
                            <label for="name" class="form-label">Sub-Breed</label>
                            <select
                              v-model="dog.subBreed"
                              class="form-select"
                              name="breed"
                              id="breed"
                            >
                              <option value>Sub-Breed</option>
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
                              name="inlineRadioOptions"
                              id="inlineRadio1"
                              value="option1"
                              v-model="dog.gender"
                            />
                            <label class="form-check-label" for="inlineRadio1">Male</label>
                          </div>
                          <div class="form-check form-check-inline">
                            <input
                              class="form-check-input"
                              type="radio"
                              name="inlineRadioOptions"
                              id="inlineRadio2"
                              value="option2"
                              v-model="dog.gender"
                            />
                            <label class="form-check-label" for="inlineRadio2">Female</label>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- <div class="card border-male">
                  <img class="dog-image border-male" src="https://via.placeholder.com/150" alt />
                  <h5 class="card-header bg-male">Dog Name</h5>
                  <div class="card-body">
                    <div class="row">
                      <div class="col-12 mb-3">
                        <label for="name" class="form-label">Name</label>
                        <input type="text" class="form-control" id="name" />
                      </div>
                      <div class="col-12 mb-3">
                        <div class="row">
                          <div class="col-6">
                            <label for="name" class="form-label">Breed</label>
                            <select class="form-select" name="breed" id="breed">
                              <option value>Breed</option>
                            </select>
                          </div>
                          <div class="col-6">
                            <label for="name" class="form-label">Sub-Breed</label>
                            <select class="form-select" name="breed" id="breed">
                              <option value>Sub-Breed</option>
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
                              name="inlineRadioOptions"
                              id="inlineRadio1"
                              value="option1"
                            />
                            <label class="form-check-label" for="inlineRadio1">Male</label>
                          </div>
                          <div class="form-check form-check-inline">
                            <input
                              class="form-check-input"
                              type="radio"
                              name="inlineRadioOptions"
                              id="inlineRadio2"
                              value="option2"
                            />
                            <label class="form-check-label" for="inlineRadio2">Female</label>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>-->
              </div>

              <div class="row">
                <div class="col-12 mt-3 d-flex justify-content-end">
                  <button class="btn btn-success">+ Add Dog</button>
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