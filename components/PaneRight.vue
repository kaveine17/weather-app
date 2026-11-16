<script setup>
import Error from './Error.vue'
import Stat from './Stat.vue';
import DayCard from './DayCard.vue'
import CitySelect from './CitySelect.vue'
import { computed, inject } from 'vue';
const { error, data, activeIndex } = defineProps({
    error: Object,
    data: Object,
    activeIndex: Number
})

const emit = defineEmits(['select-index', 'select-city'])
const errorMap = new Map([
    [1006, 'Указанный город не найден']
])
const errorDisplay = computed(() => {
    return errorMap.get(error?.error?.code)
})

const statData = computed(() => {
    if (!data) {
        return []
    }

    return [{
        label: 'Влажность',
        stat: data.forecast.forecastday[activeIndex].day.avghumidity + '%',
    },
    {
        label: 'Облачность',
        stat: data.forecast.forecastday[activeIndex].day.daily_chance_of_rain + '%',
    },
    {
        label: 'Ветер',
        stat: data.forecast.forecastday[activeIndex].day.maxwind_kph + ' км/ч',
    }]

})
</script>

<template>
    <div class="right">
        <Error v-if="error" :error="errorDisplay" />
        <div v-if="data" class="stat-data">
            <div class="stat-list">
                <Stat v-for="item in statData" v-bind="item" :key="item.label" />
            </div>
            <div class="day-card__list">
                <DayCard 
                v-for="item, i in data.forecast.forecastday" 
                :weather-code="item.day.condition.code" 
                :temp="item.day.avgtemp_c" 
                :date="new Date(item.date)"
                :key="item.date" 
                :is-active="activeIndex == i"
                @click="() => (emit('select-index', i))"></DayCard>
            </div>
        </div>
        <CitySelect/>
    </div>
</template>

<style scoped>
.right {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    gap: 32px;
    width: 560px;
    max-width: 100%;
    height: 540px;
    margin-left: -48px;
    background: var(--color-bg-main);
    padding: 50px 40px 40px 80px;
    border-radius: 25px;
}

.day-card__list {
    display: flex;
    gap: 8px;
}

.stat-data {
    display: flex;
    flex-direction: column;
    gap: 40px;
    min-width: 0;
}

.stat-list {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

@media (max-width: 900px) {
    .right {
        width: 100%;
        height: auto;
        margin-left: 0;
        margin-top: -40px;
        padding: 56px 28px 28px;
        border-radius: 25px;
    }

    .stat-data {
        gap: 28px;
    }
}

@media (max-width: 480px) {
    .right {
        margin-top: 16px;
        padding: 24px 16px 20px;
        border-radius: 20px;
        gap: 24px;
    }

    .stat-data {
        gap: 20px;
    }
}
</style>