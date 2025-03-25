<template>
    <div>
        <h2>Игра</h2>
        <div class="game-area">
            <div v-for="(target, index) in targets" class="target" :style="target" @click="hitTarget(index)">
            </div>
        </div>
        <div>Осталось времени: {{ timeLeft }} сек.</div>
        <button @click="surrender">Сдаться</button>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const props = defineProps({
    settings: Object
});

const emit = defineEmits(['end-game']);

const targets = ref([]);
const timeLeft = ref(props.settings.gameTime);
const gameInterval = ref(null);
const spawnInterval = ref(null);

const result = ref(0);

function spawnTarget() {
    targets.value = [];
    for (let index = 0; index < 10; index++) {
        targets.value.push({
            width: `${props.settings.size}px`,
            height: `${props.settings.size}px`,
            left: `${Math.random() * (500 - props.settings.size)}px`,
            top: `${Math.random() * (500 - props.settings.size)}px`,
            display: 'block'
        });
    }
}

function hitTarget(index) {
    targets.value[index].display = "none";
    result.value += 1;
}

function endGame() {
    clearInterval(gameInterval.value);
    clearInterval(spawnInterval.value);
    emit('end-game', result.value);
    result.value = 0;
    targets.value = [];
}


function surrender() {
    endGame();
}

onMounted(() => {
    if (!props.settings) {
        console.error('Настройки игры не выбраны!');
        return;
    }
    spawnInterval.value = setInterval(spawnTarget, 1000 * props.settings.speed);
    gameInterval.value = setInterval(() => {
        timeLeft.value -= 1;
        if (timeLeft.value <= 0) {
            endGame();
        }
    }, 1000);
});

onUnmounted(() => {
    clearInterval(gameInterval.value);
    clearInterval(spawnInterval.value);
});
</script>

<style scoped>
.game-area {
    position: relative;
    width: 500px;
    height: 500px;
    border: 1px solid #000;
}

.target {
    position: absolute;
    background-color: red;
    cursor: pointer;
}
</style>
