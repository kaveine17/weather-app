<script setup>
import { computed } from 'vue';
import IconCloud from '../icons/weather/IconCloud.vue';
import IconRain from '../icons/weather/IconRain.vue';
import IconSun from '../icons/weather/IconSun.vue';

const {weatherCode, temp, date, isActive} = defineProps({
    weatherCode: Number,
    temp: Number,
    date: Date,
    isActive: Boolean,

    
})


const iconColor = computed(() => {
    return isActive ? 'var(--color-primary-inverted)' : 'var(--color-primary)'
})
</script>
<template>
    <button class="day-card" :class="{active : isActive}">
        <IconSun v-if="weatherCode <= 1003" :color="iconColor""/>
        <IconRain v-else-if="weatherCode >= 1006 && weatherCode <= 1063" :color="iconColor"/>
        <IconCloud v-else-if="weatherCode >= 1063" :color="iconColor"/>
        <div class="day-card__day">
            {{ date.toLocaleDateString('ru-RU', {weekday : 'short'}) }}
        </div>
        <div class="day-card__temp">
            {{ temp }} °C 
        </div>
    </button>    
</template>

<style scoped>
    .day-card {
        width: 100%;
        min-width: 0;
        display: flex;
        flex-direction: column;
        background: var(--color-bg-card);
        padding: 20px 12px;
        align-items: center;
        justify-content: center;
        gap: 15px;
        cursor: pointer;
        border: none;
        border-radius: 10px;
        color: var(--color-primary);
        transition: background-color 0.2s ease;
    }
    .day-card:not(.active):hover {
        background-color: #3A434F;
    }

    .day-card__temp {
        font-size: clamp(14px, 2vw, 20px);
        font-weight: 700;
        white-space: nowrap;
    }
    .day-card__day {
        font-size: clamp(14px, 2vw, 20px);
    }

    @media (max-width: 480px) {
        .day-card {
            padding: 12px 6px;
            gap: 8px;
        }
    }
    .active {
        background-color: var(--color-primary);
        color: var(--color-primary-inverted);
    }
</style>