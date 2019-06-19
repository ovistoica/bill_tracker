<template>
  <main>
    <AddCategory v-if="shouldShowAddCategory" v-on:addCategory="addCategory"/>
    <div v-else>
      <AddBill v-if="shouldShowAddBill" :categories=categories v-on:addBill="addBill"/>
      <div v-else>
        <NavBar :categories=categories v-on:triggerShowAddCategory="triggerShowAddCategory" v-on:setActiveCategory="setActiveCategory"
                v-on:clearActiveCategory="clearActiveCategory" />
        <div class="container w-auto flex">
          <div class="w-1/2">
            <BillsTable :bills="activeBills" v-on:triggerShowAddBill="triggerShowAddBill" v-on:removeBill="removeBill" />
          </div>
          <div class="w-1/2">
            <Chart class="w-full m-2" :bills="activeBills"/>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import Vue from 'vue';
import AddCategory from './components/AddCategory.vue'
import AddBill from './components/AddBill.vue'
import NavBar from './components/NavBar.vue'
import Chart from './components/Chart.vue'
import BillsTable from './components/BillsTable.vue'
Vue.use(require('vue-moment'));

export default {
  name: 'app',
  components: {
    AddCategory,
    AddBill,
    Chart,
    BillsTable,
    NavBar
  },
  data() {
    return {
      bills: [],
      categories: [],
      shouldShowAddCategory: false,
      shouldShowAddBill: true,
      activeCategory: ''
    }
  },
  computed: {
    activeBills() {
      return this.bills
        .filter(
          bill => 
            this.activeCategory ? bill.category === this.activeCategory : true)
            .sort((a, b) => (new Date(a.date) < new Date(b.date) ? 1 : -1))
    }
  },
  methods: {
    addCategory(category) {
      this.categories.push(category)
      this.shouldShowAddCategory = false
    },
    triggerShowAddCategory() {
      this.shouldShowAddCategory = true
    },

    triggerShowAddBill() {
      this.shouldShowAddBill = true
    },
    addBill(bill) {
      this.bills.push(bill);
      this.shouldShowAddBill = false;
    },
    removeBill(index) {
      this.bills = this.bills.slice(0, index).concat(this.bills.slice(index + 1, this.bills.length));
    },
    setActiveCategory(category) {
      this.activeCategory = category.trim();
    }, 
    clearActiveCategory() {
      if (this.activeCategory) {
        this.activeCategory = '';
      }
    }
  },
  watch: {
    categories() {
      localStorage.setItem('categories', JSON.stringify(this.categories))
    },
    bills() {
      localStorage.setItem('bills', JSON.stringify(this.bills));
    }
  },
  mounted() {
    if (localStorage.getItem('categories')) {
      this.categories = JSON.parse(localStorage.getItem('categories'))
    }

    if (localStorage.getItem('bills')) {
      this.bills = JSON.parse(localStorage.getItem('bills'));
    }

    if (!this.categories.length && !this.bills.length) {
      this.shouldShowAddCategory = true
    }
  }
}
</script>
