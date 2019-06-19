<template>
    <ul class="list-reset inline flex justify-center border-b-4 mb-0">
        <li class="p-4 inline hover:bg-grey-light uppercase font-black cursor-pointer" @click="clearActiveCategory()">All</li>
        <li 
            class="p-4 inline hover:bg-grey-light uppercase font-black cursor-pointer"
            :class="[ activeCategory === category ? 'bg-grey-dark' : 'bg-grey-lighter' ]"
            v-for="category in categories"
            :key="category"
            @click="setActiveCategory(category)">
            {{category}}
        </li>
        <li class="p-4 inline bg-grey-lighter hover:bg-grey-light uppercase font-black cursor-pointer" 
            @click="triggerShowAddCategory">➕</li>

    </ul>
</template>

<script>
export default {
    props: ['categories', 'activeCategory'],
    methods: {
        triggerShowAddCategory() {
            this.$emit('triggerShowAddCategory');
        },
        setActiveCategory(category) {
            this.$emit('setActiveCategory', category);
        },
        clearActiveCategory() {
            this.$emit('clearActiveCategory');
        }
    }
}
</script>
