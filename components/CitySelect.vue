<script setup>
import Button from './Button.vue';
import IconLocation from '../icons/IconLocation.vue';
import Input from './Input.vue';
import { inject, ref} from 'vue';
import { cityProvide } from '../src/constans.js';


//citySelection


const isEdited = ref(false)

const city = inject(cityProvide)
let inputValue = ref(city.value)

function select() { 
    isEdited.value = false
    city.value = inputValue.value
}

function edit() { 
    isEdited.value = true
    inputValue.value = city.value 
}

</script>

<template>
    <div class="city-select">
        <div v-if="isEdited" class="city-input">
            <Input 
            v-focus
            v-model="inputValue" 
            placeholder="Введите город"
            @keyup.enter='select()'
            />
            <Button @click="select()" :class="{ 'button--auto': isEdited }"">
                Сохранить 
            </Button>
        </div>
        <div v-else>
            <Button @click="edit()">
                <IconLocation/> 
            Изменить город 
            </Button>
        </div>
    </div>
</template>

<style scoped>
.city-input {
    display: flex;
    gap: 12px;
    width: 100%;
}
.city-select {
    width: 100%;
    min-width: 0;
}

@media (max-width: 480px) {
    .city-input {
        flex-direction: column;
    }
}
</style>