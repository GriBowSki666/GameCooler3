<template>
    <div>
        <h2>Статистика</h2>
        <div class="filters">
            <label>Скорость целей:
                <select v-model="filter.speed">
                    <option value="">Все</option>
                    <option v-for="option in speedOptions" :value="option">{{ option }}</option>
                </select>
            </label>
            <label>Раз мер целей:
                <select v-model="filter.size">
                    <option value="">Все</option>
                    <option v-for="option in sizeOptions" :value="option">{{ option }}</option>
                </select>
            </label>
            <label>Время игры:
                <select v-model="filter.gameTime">
                    <option value="">Все</option>
                    <option v-for="option in timeOptions" :value="option">{{ option }}</option>
                </select>
            </label>
        </div>
        <table class="stats-table">
            <thead>
                <tr>
                    <th>Скорость целей</th>
                    <th>Размер целей</th>
                    <th>Время игры</th>
                    <th>Очки</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(game) in filteredGames">
                    <td>{{ game.settings.speed }}</td>
                    <td>{{ game.settings.size }}</td>
                    <td>{{ game.settings.gameTime }}</td>
                    <td>{{ game.result }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
    games: {
        type: Array,
        required: true
    }
});


const speedOptions = [1, 2, 3];
const sizeOptions = [30, 50, 70];
const timeOptions = [10, 20, 30];

const filter = ref({
    speed: '',
    size: '',
    gameTime: ''
});

const filteredGames = computed(() => {
    if (!props.games || props.games.length === 0) {
        return [];
    }

    return props.games.filter(game => {
        return (
            (filter.value.speed === '' || game.settings.speed === filter.value.speed) &&
            (filter.value.size === '' || game.settings.size === filter.value.size) &&
            (filter.value.gameTime === '' || game.settings.gameTime === filter.value.gameTime)
        );
    });
});

</script>

<style scoped>
.filters {
    margin-bottom: 20px;
}

.filters label {
    margin-right: 15px;
}

.filters select {
    padding: 5px;
    font-size: 14px;
}

.stats-table {
    width: 100%;
    border-collapse: collapse;
}

.stats-table th,
.stats-table td {
    border: 1px solid #740c0c;
    padding: 8px;
    text-align: center;
}

.stats-table th {
    background-color: #432e2e;
    font-weight: bold;
}

.stats-table tr:nth-child(even) {
    background-color: #5d1b1b;
}

.stats-table tr:hover {
    background-color: #ab1a1a;
}
</style>
