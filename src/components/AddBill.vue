<template>
  <div class="h-100 w-full flex items-center justify-center font-sans">
    <div class="bg-white rounded shadow p-6 m-4 w-full lg:w-3/4 lg:max-w-lg">
      <div class="mb-4">
        <h1 class="text-grey-darkest">Enter a new bill</h1>
        <p></p>
        <div class="flex mt-4">
          <datepicker wrapper-class="shadow appearance-none border rounded text-grey-darker" input-class="w-full w-full py-2 px-3 mr-4 " v-model="date"></datepicker>

          <select v-model="category">
            <option v-for="category in categories" :value="category" :key="category">
              {{ category }}
            </option>
          </select>

          <input class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-grey-darker" placeholder="Set amount" v-model="amount">
          <button class="flex-no-shrink p-2 border-2 rounded bg-teal text-white border-teal hover:text-white hover:bg-teal" @click="handleClick">Add</button>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import Datepicker from 'vuejs-datepicker'

export default {
    name: 'AddBill',
    props: ['categories'],
    data: function() {
        return {
            date: new Date(),
            category: this.categories[0],
            amount: 0
        }
    },
    components: {
        Datepicker
    },
    methods: {
        handleClick: function() {
            if (!this.date) {
                alert('Enter a date');
                return;
            }

            if (!this.amount) {
                alert('Enter an amount');
                return;
            }

            this.$emit('addBill', {
                date: this.date,
                category: this.category,
                amount: this.amount
            });
        },
    }
}
</script>
