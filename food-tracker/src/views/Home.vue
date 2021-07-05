<template>
  <AddFood v-show="showAddFood" @add-food="addFood" />
  <Foods
    @toggle-reminder="toggleReminder"
    @delete-food="deleteFood"
    :foods="foods"
  />
</template>
<script>
import Foods from '../components/Foods'
import AddFood from '../components/AddFood'
export default {
  name: 'Home',
  props: {
    showAddFood: Boolean,
  },
  components: {
    Foods,
    AddFood,
  },
  data() {
    return {
      foods: [],
    }
  },
  methods: {
    async addTask(food) {
      const res = await fetch('api/foods', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(food),
      })
      const data = await res.json()
      this.foods = [...this.foods, data]
    },
    async deleteFood(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/foods/${id}`, {
          method: 'DELETE',
        })
        res.status === 200
          ? (this.foods = this.foods.filter((food) => food.id !== id))
          : alert('Error deleting food')
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchFood(id)
      const updFood = { ...foodToToggle, reminder: !foodToToggle.reminder }
      const res = await fetch(`api/foods/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updFood),
      })
      const data = await res.json()
      this.foods = this.foods.map((food) =>
        food.id === id ? { ...food, reminder: data.reminder } : food
      )
    },
    async fetchFoods() {
      const res = await fetch('api/foods')
      const data = await res.json()
      return data
    },
    async fetchFood(id) {
      const res = await fetch(`api/foods/${id}`)
      const data = await res.json()
      return data
    },
  },
  async created() {
    this.tasks = await this.fetchFoods()
  },
}
</script>
