<template>
    <div id="sidebar">
        <div id="search-container">
            <input
                type="text"
                v-model="query"
                @input="onSearch"
                placeholder="Nhập tên phù phép..."
            />
        </div>
        <div class="section" v-for="section in sections" :key="section">
            <div class="section-title">{{ section.title }}</div>
            <input
                type="text"
                v-model="section.query"
                @input="() => onFilter(section.filter)"
                placeholder="Tìm kiếm..."
            />
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            query: '',
            sections: [
                { title: 'Tất cả trang bị', filter: 'all', query: '' },
                { title: 'Vũ khí', filter: 'weapons', query: '' },
                { title: 'Giày', filter: 'boots', query: '' },
                { title: 'Kiếm', filter: 'swords', query: '' },
                { title: 'Rìu', filter: 'axes', query: '' },
                { title: 'Giáp', filter: 'armor', query: '' }
            ]
        }
    },
    methods: {
        onSearch() {
            this.$emit('search', this.query)
        },
        onFilter(filter) {
            const section = this.sections.find((s) => s.filter === filter)
            this.$emit('filter', section.query)
        }
    }
}
</script>

<style>
#sidebar {
    width: 250px;
    background-color: #f4f4f4;
    padding: 20px;
    box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
}

.section-title {
    font-size: 18px;
    font-weight: bold;
    margin: 10px 0;
}

input {
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
    font-size: 16px;
}
</style>
