<template>
    <table v-if="results.length" class="table table-striped table-bordered">
        <thead>
            <tr>
                <th @click="sortBy('name')" class="sortable">
                    Tên
                    <i :class="getSortIcon('name')" class="bi"></i>
                </th>
                <th>Mô tả</th>
                <th>Trang bị</th>
                <th @click="sortBy('rarity')" class="sortable">
                    Độ hiếm
                    <i :class="getSortIcon('rarity')" class="bi"></i>
                </th>
                <th>Cấp độ tối đa</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="enchant in sortedResults" :key="enchant.name">
                <td v-html="highlightText(enchant.name)"></td>
                <td>{{ enchant.description }}</td>
                <td>{{ enchant.applicable_items }}</td>
                <td :class="getRarityClass(enchant.rarity)">{{ enchant.rarity }}</td>
                <td>{{ enchant.max_level }}</td>
            </tr>
        </tbody>
    </table>
    <p v-else>Không tìm thấy kết quả</p>
</template>

<script>
export default {
    props: {
        results: {
            type: Array,
            required: true
        },
        query: {
            type: String,
            required: true
        }
    },
    data() {
        return {
            currentSort: 'name',
            currentSortDir: 'asc'
        }
    },
    computed: {
        sortedResults() {
            return this.results.slice().sort((a, b) => {
                let modifier = 1
                if (this.currentSortDir === 'desc') modifier = -1
                if (this.currentSort === 'rarity') {
                    const rarityOrder = [
                        'simple',
                        'unique',
                        'elite',
                        'ultimate',
                        'legendary',
                        'fabled'
                    ]
                    return (
                        modifier *
                        (rarityOrder.indexOf(a[this.currentSort].toLowerCase()) -
                            rarityOrder.indexOf(b[this.currentSort].toLowerCase()))
                    )
                }
                if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier
                if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier
                return 0
            })
        }
    },
    methods: {
        highlightText(text) {
            const query = this.query
            if (!query) return text
            const regex = new RegExp(`(${query})`, 'gi')
            return text.replace(regex, '<span class="highlight">$1</span>')
        },
        getRarityClass(rarity) {
            switch (rarity.toLowerCase()) {
                case 'fabled':
                    return 'rarity-fabled'
                case 'legendary':
                    return 'rarity-legendary'
                case 'ultimate':
                    return 'rarity-ultimate'
                case 'elite':
                    return 'rarity-elite'
                case 'unique':
                    return 'rarity-unique'
                default:
                    return ''
            }
        },
        sortBy(key) {
            if (this.currentSort === key) {
                this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
            } else {
                this.currentSort = key
                this.currentSortDir = 'asc'
            }
        },
        getSortIcon(key) {
            if (this.currentSort === key) {
                return this.currentSortDir === 'asc' ? 'bi-chevron-up' : 'bi-chevron-down'
            }
            return 'bi-chevron-expand'
        }
    }
}
</script>

<style scoped>
table {
    width: 100%;
}

.highlight {
    background-color: yellow;
}

.sortable {
    cursor: pointer;
    user-select: none;
}

.bi {
    margin-left: 0.5em;
}

.rarity-fabled {
    color: purple;
    font-weight: bold;
}

.rarity-legendary {
    color: orange;
}

.rarity-ultimate {
    color: gold;
}

.rarity-elite {
    color: aqua;
}

.rarity-unique {
    color: green;
}
</style>
