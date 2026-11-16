<script setup>
    const data = defineModel({type: String, required : true})

</script>
<template>
    <input 
    class="input" 
    type="text"
    v-model="data"
    >
</template>

<style scoped>
    .input { 
        color: var(--color-primary);
        border-radius: 10px;
        width: 100%;
        min-width: 0;
        flex: 1;
        background: var(--color-bg-card);
        box-shadow: 1px 2px 4px 0px var(--color-bg-main);
        border: none;
        font-size: 18px;
        padding: 15px 20px;
        height: 53px;
    }
    .input::placeholder {
        color : var(--color-text-placeholder)
    }

</style>