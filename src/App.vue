<script setup>
import PaneRight from '../components/PaneRight.vue';
import { onMounted, provide, ref, watch} from 'vue';
import { cityProvide, API_ENDPOINT } from './constans.js';
import PaneLeft from '../components/PaneLeft.vue';



let data = ref()
let error = ref()
let activeIndex = ref(0);
const city = ref('Moscow ')
provide(cityProvide, city)


watch(city, () => {
    getCity(city.value)

})

onMounted(() => {
    getCity(city.value)
})

async function getCity(city) {
    const params = new URLSearchParams({
        q: city,
        lang: 'ru',
        key: API_KEY,
        days: 3,
    })
    const res = await fetch(`${API_ENDPOINT}/forecast.json?${params.toString()}`)
    if (res.status != 200) {
        error.value = await res.json()
        data.value = null
        return;
    }
    error.value = null
    data.value = await res.json()
}
</script>

<template>
    <main class="main">
        <PaneLeft
            :data
            :active-index="activeIndex"
        />
        <PaneRight
            :data
            :error
            :active-index="activeIndex"
            @select-index="(i) => (activeIndex = i)"
        />
    </main>
</template>

<style scoped>
.main {
    display: flex;
    align-items: center;
    justify-content: center;
    width: min(980px, 100%);
}

@media (max-width: 900px) {
    .main {
        flex-direction: column;
        align-items: stretch;
        max-width: 480px;
    }
}
</style>