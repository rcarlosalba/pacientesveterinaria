<script setup>
import {computed } from 'vue'
 const props = defineProps({
    alertData: {
        type: Object, 
        required: true,
    }
 })

 const isError = computed(()=>{
    return props.alertData.tipo === 'error'
 })

</script>

<template>
    <div
        :class="[isError ? 'bg-red-500' : 'bg-green-500']" 
        class="text-white text-center p-3 mb-3 uppercase font-bold rounded-md" 
    >
        <p>{{ alertData.mensaje }}</p>
    </div>
</template>

<style scope>
 
</style>