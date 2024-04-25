<template>
    <div class="card inline">
        <h3>{{ firstName }}</h3>
        <button class="btn danger" @click="$emit('remove')">удалить</button>
    </div>
</template>

<script>
export default {
    emits: ['remove'],
    props: ['firstName']
}
</script>

<style scoped>
.inline {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
</style>