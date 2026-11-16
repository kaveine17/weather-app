<script setup>
import { computed } from 'vue';
import IconLocation from '../icons/IconLocation.vue';
import IconCloud from '../icons/weather/IconCloud.vue';
import IconRain from '../icons/weather/IconRain.vue';
import IconSun from '../icons/weather/IconSun.vue';

const { data, activeIndex } = defineProps({
    data: Object,
    activeIndex: Number,
})

const selectedDay = computed(() => data?.forecast?.forecastday?.[activeIndex])

const weekday = computed(() => {
    if (!selectedDay.value) {
        return ''
    }
    const label = new Date(selectedDay.value.date).toLocaleDateString('ru-RU', { weekday: 'long' })
    return label.charAt(0).toUpperCase() + label.slice(1)
})

const dateLabel = computed(() => {
    if (!selectedDay.value) {
        return ''
    }
    return new Date(selectedDay.value.date).toLocaleDateString('ru-RU', {
        day: 'numeric',
        month: 'short',
        year: 'numeric',
    })
})

const locationLabel = computed(() => {
    if (!data?.location) {
        return ''
    }
    return `${data.location.name}, ${data.location.country}`
})

const isToday = computed(() => activeIndex === 0 && data?.current)

const weatherCode = computed(() => {
    if (isToday.value) {
        return data.current.condition.code
    }
    return selectedDay.value?.day?.condition?.code
})

const temp = computed(() => {
    if (isToday.value) {
        return Math.round(data.current.temp_c)
    }
    return Math.round(selectedDay.value?.day?.avgtemp_c ?? 0)
})

const conditionText = computed(() => {
    if (isToday.value) {
        return data.current.condition.text
    }
    return selectedDay.value?.day?.condition?.text ?? ''
})
</script>

<template>
    <div class="left">
        <div v-if="data" class="left-content">
            <div class="left-header">
                <div class="left-weekday">{{ weekday }}</div>
                <div class="left-date">{{ dateLabel }}</div>
                <div class="left-location">
                    <IconLocation class="left-location__icon" />
                    <span>{{ locationLabel }}</span>
                </div>
            </div>
            <div class="left-weather">
                <IconSun v-if="weatherCode <= 1003" :size="80" />
                <IconRain v-else-if="weatherCode >= 1006 && weatherCode <= 1063" :size="80" />
                <IconCloud v-else-if="weatherCode >= 1063" :size="80" />
                <div class="left-temp">{{ temp }} °C</div>
                <div class="left-condition">{{ conditionText }}</div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.left {
    position: relative;
    z-index: 1;
    isolation: isolate;
    width: 420px;
    max-width: 100%;
    height: 640px;
    flex-shrink: 0;
    border-radius: 25px;
    overflow: hidden;
    color: var(--color-primary);
}

.left::before {
    content: '';
    position: absolute;
    inset: -20px;
    background-image: url('../icons/weather/back.png');
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center;
    z-index: -1;
}

.left-content {
    position: relative;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 48px 40px 56px;
}

.left-weekday {
    font-size: clamp(28px, 5vw, 42px);
    font-weight: 700;
    line-height: 1.2;
}

.left-date {
    margin-top: 8px;
    font-size: clamp(14px, 2vw, 18px);
    font-weight: 500;
    opacity: 0.9;
}

.left-location {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-top: 18px;
    font-size: clamp(14px, 2vw, 16px);
    font-weight: 500;
    min-width: 0;
}

.left-location span {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}

.left-location__icon {
    width: 16px;
    height: 20px;
    flex-shrink: 0;
}

.left-weather {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 8px;
}

.left-temp {
    font-size: clamp(36px, 6vw, 48px);
    font-weight: 700;
    line-height: 1.1;
}

.left-condition {
    font-size: clamp(20px, 4vw, 28px);
    font-weight: 500;
}

@media (max-width: 900px) {
    .left {
        width: 100%;
        height: min(72vh, 560px);
    }

    .left-content {
        padding: 36px 28px 40px;
    }
}

@media (max-width: 480px) {
    .left {
        height: 440px;
        border-radius: 20px;
    }

    .left-content {
        padding: 28px 24px 32px;
    }
}
</style>
