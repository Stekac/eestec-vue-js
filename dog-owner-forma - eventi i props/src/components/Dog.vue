<script setup>
import { computed } from 'vue';
const { dog, breeds } = defineProps(['dog', 'breeds'])

const listOfBreeds = computed(() => Object.keys(breeds));

const hasSubBreed = (breed) => breeds[breed]?.length > 0;

const getSubBreed = (breed) => breeds[breed];

const dogNameExists = (dogName) => {
  return dogName !== ''
};

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

</script>

<template>
  <div class="card mb-3">
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
                <option v-for="subBreed in getSubBreed(dog.breed)" :value="subBreed">{{ subBreed }}</option>
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
</template>