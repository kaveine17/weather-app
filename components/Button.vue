<script setup></script>

<template>
    
    <button class="button">
      <slot/>
    </button>
    </template>

<style scoped>
  .button { 
    display: flex;
    border: none;
    border-radius: 10px;
    background: var(--gradient);
    padding: 10px 24px;
    font-size: clamp(16px, 2vw, 20px);
    height: 53px;
    flex: 0 0 auto;
    font-weight: 600;
    color: var(--color-primary);
    align-items: center;
    gap: 9px;
    width: 100%;
    justify-content: center;
    
  }
  .button:hover { 
    background:var(--gradient-inverted);
    cursor: pointer;
  }
  .button--auto {
    width: auto;
}
</style>

