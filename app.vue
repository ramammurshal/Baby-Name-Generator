<script setup lang="ts">
import { Gender, Popularity, Length, names } from "@/data";

interface OptionsState {
  gender: Gender;
  popularity: Popularity;
  length: Length;
};

// default state
const options = reactive<OptionsState>({
  gender: Gender.BOY,
  popularity: Popularity.TRENDY,
  length: Length.SHORT,
});

const computeSelectedNames = () => {
  const filteredNames = names
    .filter(name => name.gender === options.gender)
    .filter(name => name.popularity === options.popularity)
    .filter(name => {
      if (name.length === Length.ALL) return true
      else return name.length === options.length
    });
  
  selectedNames.value = filteredNames.map(name => name.name);
};

const selectedNames = ref<string[]>([]);

const removeName = (index) => {
  const filteredNames = [ ...selectedNames.value ];
  filteredNames.splice(index, 1);
  selectedNames.value = filteredNames;
}

const optionsArray = [
  {
    title: '1) Choose a gender',
    category: 'gender',
    buttons: [Gender.BOY, Gender.GIRL, Gender.UNISEX]
  },
  {
    title: "2) Choose the name's popularity",
    category: 'popularity',
    buttons: [Popularity.UNIQUE, Popularity.TRENDY]
  },
  {
    title: "3) Choose name's length",
    category: 'length',
    buttons: [Length.SHORT, Length.LONG, Length.ALL]
  }
];
</script>

<template>
  <!-- container -->
  <div class="container">
    
    <!-- title -->
    <h1>Baby Name Generator</h1>
    <p>Choose your options and click the "Find Names" buttom below</p>
    <!-- title -->

    <!-- options-container -->
    <div class="options-container">

      <!-- options-component -->
      <Option 
        v-for="option in optionsArray" 
        :key="option.title"
        :option="option"
        :options="options"
      />
      <!-- ./options-component -->

      <!-- search-button -->
      <button 
        class="primary"
        @click="computeSelectedNames"
      >
        Find Names
      </button>
      <!-- ./search-button -->

    </div>
    <!-- ./options-container -->

    <!-- result -->
    <div class="cards-container">
      <CardName 
        v-for="(name, index) in selectedNames" 
        :key="name" 
        :name="name"
        @remove="removeName"
        :index="index"
      />
    </div>
    <!-- ./result -->

  </div>
  <!-- ./container -->
</template>

<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}
h1 {
  font-size: 3rem;
}
.options-container {
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}
.primary {
  background-color: rgb(249, 87, 89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
  transition: all 0.4s ease;
}
.primary:hover {
  background-color: rgb(225, 82, 84);
}
.cards-container {
  margin-top: 3rem;
  display: flex;
  flex-wrap: wrap;
}
</style>
