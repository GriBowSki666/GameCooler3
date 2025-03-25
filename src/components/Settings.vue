<template>
    <div>
        <h2>Настройки игры</h2>
        <div>
            <label>Скорость целей:</label>
            <select v-model="settings.speed">
                <option v-for="option in speedOptions" :value="option">{{ option }}</option>
            </select>
        </div>
        <div>
            <label>Размер целей:</label>
            <select v-model="settings.size">
                <option v-for="option in sizeOptions" :value="option">{{ option }}</option>
            </select>
        </div>
        <div>
            <label>Время игры (сек.):</label>
            <select v-model="settings.gameTime">
                <option v-for="option in timeOptions" :value="option">{{ option }}</option>
            </select>
        </div>
        <button @click="startGame">Начать игру</button>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const emit = defineEmits(['start-game']);

const speedOptions = [1, 2, 3];
const sizeOptions = [30, 50, 70];
const timeOptions = [10, 20, 30];

const settings = ref({
    speed: speedOptions[0],
    size: sizeOptions[0],
    gameTime: timeOptions[0]
});

function startGame() {
    emit('start-game', settings.value);
}
</script>

<style scoped>
div {
    margin-bottom: 10px;
}

label {
    margin-right: 10px;
}

select {
    padding: 5px;
    font-size: 16px;
}

button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
}
</style>