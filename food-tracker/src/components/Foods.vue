<template>
  <div :key="food.id" v-for="food in foods">
    <Food
      @toggle-reminder="$emit('toggle-reminder', food.id)"
      @delete-food="$emit('delete-food', task.id)"
      :food="food"
    />
  </div>
</template>

<script>
import Food from './Food.vue'
export default {
  name: 'Foods',
  props: {
    foods: Array,
  },
  components: {
    Food,
  },
  emits: ['delete-food', 'toggle-reminder'],
}
</script>