<template>
    <table class="w-full h-full p-5 m-1">
        <thead class="bg-teal-500 text-white text-center">
            <tr>
                <th scope="col">Date</th>
                <th scope="col">Amount</th>
                <th scope="col">Category</th>
                <th scope="col"></th>
            </tr>
        </thead>
        <tbody>
            <tr class="p-4 bg-teal-800 text-center text-white">
                <td colspan="4">
                    <button class="underline" @click="triggerShowAddBill">Add new </button>
                </td>
            </tr>
            <tr v-for="(bill, index) in bills" :key="index" class="p-4">
                <td>{{bill.date | moment("MMM D YYYY")}}</td>
                <td>{{bill.amount}}</td>
                <td>{{bill.category}}</td>
                <td><button @click="removeBill(index)">𝗫</button></td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    name: 'BillsTable',
    props: ['bills'],
    methods: {
        triggerShowAddBill: function() {
            this.$emit('triggerShowAddBill');
        },
        removeBill: function(index) {
            this.$emit('removeBill', index);
        }
    },
}
</script>

