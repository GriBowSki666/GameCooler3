<template>
    <div>
        <div class="navigation">
            <button @click="switchView('settings')"
                :disabled="currentView === 'settings' || isGameStarted">Настройки</button>
            <button @click="switchView('game')"
                :disabled="currentView === 'game' || !gameSettings || isGameStarted">Игра</button>
            <button @click="switchView('statistics')"
                :disabled="currentView === 'statistics' || isGameStarted">Статистика</button>
        </div>
        <Settings v-if="currentView === 'settings'" @start-game="handleStartGame" />
        <Game v-else-if="currentView === 'game'" :settings="gameSettings" @end-game="handleEndGame" />
        <Statistics v-else-if="currentView === 'statistics'" :games="statistics" />
    </div>
</template>

<script setup>
import { ref } from 'vue';
import Settings from './components/Settings.vue';
import Game from './components/Game.vue';
import Statistics from './components/Statistics.vue';

const currentView = ref('settings');
const gameSettings = ref(null);
const statistics = ref([]);
const isGameStarted = ref(false);

function switchView(view) {
    currentView.value = view;
}

function handleStartGame(settings) {
    gameSettings.value = settings;
    switchView('game');
    isGameStarted.value = true;
}

function handleEndGame(result) {
    statistics.value.push({
        settings: gameSettings.value,
        result: result
    });
    isGameStarted.value = false;
    switchView('statistics');
}
</script>

<style scoped>
.navigation {
    margin-bottom: 20px;
}

.navigation button {
    margin-right: 10px;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
}

.navigation button:disabled {
    background-color: #911010;
    cursor: not-allowed;
}
</style>