<template>
    <div id="app" class="container-fluid">
        <NavBar></NavBar>
        <div class="row justify-content-center my-4">
            <div class="col-md-6">
                <input
                    type="text"
                    v-model="query"
                    class="form-control"
                    placeholder="Nhập tên phù phép..."
                    @input="filterResults"
                />
            </div>
        </div>
        <div class="row justify-content-center mb-4">
            <div class="col-md-10">
                <div class="d-flex flex-wrap justify-content-center">
                    <label v-for="item in filterItems" :key="item" class="form-check-label m-2">
                        <input
                            type="checkbox"
                            :value="item"
                            v-model="selectedFilters"
                            @change="filterResults"
                            class="form-check-input"
                        />
                        {{ item }}
                    </label>
                </div>
            </div>
        </div>
        <div class="row justify-content-center">
            <div class="col-md-10">
                <ResultTable :results="filteredResults" :query="query"></ResultTable>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import ResultTable from './components/ResultTable.vue'
import NavBar from './components/NavBar.vue'

export default {
    name: 'App',
    components: {
        ResultTable,
        NavBar
    },
    data() {
        return {
            enchantments: [],
            query: '',
            selectedFilters: [],
            filterItems: [
                'Swords',
                'Trident',
                'Armor',
                'Helmets',
                'Boots',
                'Bow',
                'Crossbow',
                'Chestplates',
                'Axes',
                'Pickaxe',
                'Leggings',
                'Weapons'
            ],
            filteredResults: []
        }
    },
    created() {
        axios
            .get('./enchants.json')
            .then((response) => {
                this.enchantments = response.data
                this.filteredResults = this.enchantments
            })
            .catch((error) => {
                console.error(error)
            })
    },
    methods: {
        filterResults() {
            const queryLower = this.query.toLowerCase()
            this.filteredResults = this.enchantments.filter((enchant) => {
                const matchesQuery = enchant.name.toLowerCase().includes(queryLower)
                const matchesFilters =
                    this.selectedFilters.length === 0 ||
                    this.selectedFilters.some((filter) =>
                        enchant.applicable_items.toLowerCase().includes(filter.toLowerCase())
                    )
                return matchesQuery && matchesFilters
            })
        }
    }
}
</script>

<style scoped>
body {
    font-family: Arial, sans-serif;
}
</style>
