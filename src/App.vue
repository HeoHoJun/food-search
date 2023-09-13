<script setup>
import { reactive } from 'vue'

const data = reactive({
  food: null
})

function process(object) {
  object.ingredients = []
  for (let i = 1; i <= 20; i++){
    if (object[`strIngredient${i}`]){
      let ingredient = {
        name: object[`strIngredient${i}`],
        measure: object[`strMeasure${i}`]
      }
      object.ingredients.push(ingredient)
    }
    else {
      break
    }
  }
  return object
}

fetch('https://www.themealdb.com/api/json/v1/1/lookup.php?i=53071')
  .then(response => response.json())
  .then(json => data.food = process(json.meals[0]))
  .catch(error => console.log(error))
</script>

<template>
  <div>
    <div v-if="data.food" class="shadow bg-gray-100 text-black rounded-xl">
      <img class="rounded-t-xl" :src="data.food.strMealThumb" alt="">
      <div class="text-4xl font-bold p-1 mt-3">{{ data.food.strMeal }}</div>
      <div class="p-4">
        <div v-for="i of data.food.ingredients" class="grid grid-cols-2">
          <div class="text-right pr-4 font-bold">{{ i.name }}</div>
          <div class="text-left pl-4">{{ i.measure }}</div>
        </div>
      </div>
    <div class="p-3">{{ data.food.strInstructions }}</div>

    </div>
  </div>
</template>