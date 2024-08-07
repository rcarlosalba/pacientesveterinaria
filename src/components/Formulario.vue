<script setup>
 import { reactive, computed } from 'vue'
 import Alerta from './Alerta.vue'


 const alertData = reactive({
    tipo: '',
    mensaje: '',
 })

 const emit = defineEmits(['update:nombre', 'update:propietario', 'update:email', 'update:fecha', 'update:sintomas', 'guardar-paciente'])

 const props = defineProps({
    nombre: String,
    propietario: String,
    email: String,
    fecha: String,
    sintomas: String,
    id: [String, null],
 })

const validarForm = () => {
    if (Object.values(props).includes('')) {
        alertData.tipo = 'error'
        alertData.mensaje = 'Todos los campos son obligatorios'
        removeAlert()
        return
    }
    emit('guardar-paciente')
    alertData.tipo = 'success'
    alertData.mensaje = 'Paciente Registrado Correctamente'
    removeAlert()
}  

const removeAlert = ()=>{
    setTimeout(() => {
        alertData.tipo = ''
        alertData.mensaje = ''
    }, 3000);
}

const editanto = computed(()=>{
    return props.id 
})
</script>

<template>
 <div class="md:w-1/2">
    <h2 class="font-bold text-3xl text-center">Seguimiento Pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">Añade Pacientes y
        <span class="text-indigo-600 font-bold">Administralos</span>
    </p>
    <Alerta 
        v-if="alertData.mensaje"
        :alertData="alertData" 
    />
    <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
        @submit.prevent="validarForm"
    >
        <div class="mb-5">
            <label for="mascota" class="block text-gray-700 uppercase font-bold">
                Nombre Mascota
            </label>
            <input type="text" id="mascota" placeholder="Nombre de la Mascota" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md" 
            :value="nombre"
            @input="$emit('update:nombre', $event.target.value)"
            >
        </div>
        <div class="mb-5">
            <label for="propietario" class="block text-gray-700 uppercase font-bold">
                Nombre del Propietario
            </label>
            <input type="text" id="propietario" placeholder="Nombre del Propietario" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
            :value="propietario"
            @input="$emit('update:propietario', $event.target.value)"
            >
        </div>
        <div class="mb-5">
            <label for="email" class="block text-gray-700 uppercase font-bold">
                Dirección de correo:
            </label>
            <input type="email" id="email" placeholder="Dirección de Correo" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
            :value="email"
            @input="$emit('update:email', $event.target.value)"
            >
        </div>
        <div class="mb-5">
            <label for="fecha" class="block text-gray-700 uppercase font-bold">
                Fecha de Alta
            </label>
            <input type="date" id="fecha" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md"
            :value="fecha"
            @input="$emit('update:fecha', $event.target.value)"
            >
        </div>
        <div class="mb-5">
            <label for="sintomas" class="block text-gray-700 uppercase font-bold">
                Síntomas
            </label>
            <textarea id="sintomas" placeholder="Síntomas de la Mascota" class="border-2 w-full p-2 mt-2 placeholder-gray-400 rounded-md h-36"
            :value="sintomas"
            @input="$emit('update:sintomas', $event.target.value)"
            ></textarea>
        </div>
        <input type="submit" 
        class="transition-colors bg-indigo-600 hover:bg-indigo-500 w-full p-3 text-white uppercase font-bold rounded-md" 
        :value="[editanto ? 'Guardar Cambios' : 'Registrar Paciente']">
    </form>
 </div>
</template>
