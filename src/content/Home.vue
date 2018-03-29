<template>
    <div id="home-content">
         <todo-component></todo-component>
     </div>
</template>

<script>

import TodoComponent from '../components/todo.vue';

export default {
    components: {
        "todo-component": TodoComponent
    }
}
</script>

<style scoped>

</style>  