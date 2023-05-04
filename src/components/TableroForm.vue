<script setup>
     const props = defineProps(['tablero']);  
</script>

<template>
   <div class="tablero-form">
        <input type="text" v-model="tablero.nombre">
        <!-- <input type="color" v-model="tablero.color"> -->
   </div>
</template>

<style scoped>
    .tablero-form {
        border : 1px solid black;
        background-color: rgb(84, 73, 102);

    }

</style>