<template>
  <div
    @dblclick="$emit('toggle-reminder', food.id)"
    :class="[food.reminder ? 'reminder' : '', 'food']"
  >
    <h3>
      {{ food.text }}
      <i @click="$emit('delete-food', food.id)" class="fas fa-times"></i>
    </h3>
     <p>{{ food.day }}</p>
  </div>
</template>
<script>
export default {
  name: 'Food',
  props: {
    food: Object,
  },
}
</script>