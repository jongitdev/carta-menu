<script setup>
    import {  ref, nextTick } from "vue";
    const emits = defineEmits(["onNewItem"]);
    const text = ref("");
    const MAX = 8;
    let quedan = ref(MAX);

    function handleKeyDown(evt) {
        if (evt.key === "Enter") {
            console.log('emit hijo')
            emits("onNewItem", text);
            text.value = "";
        }
        quedan = MAX - text.value.length;
        if (quedan <= 0)
        {
            alert('solo puedes escribir '  + MAX + ' caracteres ' + text.value);
            nextTick(() => {
                 text.value = text.value.slice(0,MAX);

            }); 
             
        }
    }
     
</script>

<template>
    <input v-model="text" @keydown="handleKeyDown" />{{ text }}
    <input  size="4" v-model="quedan" />
</template>

<style scoped></style>