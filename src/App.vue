<!-- //содержит базовый html и javascript для приложения. -->
<script setup>
import TodoList from './TodoList.vue';
</script>

<template>
    <main>
        <h1>Todo List/Лист задач</h1>
        <TodoList/>
    </main>
</template>

<style scoped>
    main {
        font-family: Verdana, sans-serif;
    }

    h1 {
        font-weight: bold; 
        color: rgb(84, 79, 79);
        padding: 8px 16px;
        background-color: rgb(212, 203, 203);
        border: solid rgb(237, 233, 233);
        border-radius: 8px;
        margin-bottom: 16px;
        text-align: center;
        font-style:italic;
    }
</style>
