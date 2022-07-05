<template>
    <button class="btn">
       <slot></slot> 
    </button>
</template>

<script>
    export default {
        name: 'my-button'
    }
</script>

<style lang="scss" scoped>
.btn { 
    padding: 5px 10px;
    color: rgb(0, 0, 0);
    background: none;
    border: 2px solid rgba(71, 71, 71, 0.877);
    }
</style>