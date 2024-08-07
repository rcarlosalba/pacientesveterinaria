<script setup>
import { reactive, ref, watch, computed, onMounted } from 'vue'
import { uid } from 'uid'
import Header from './components/Header.vue'
import Formulario from './components/Formulario.vue'
import PacienteList from './components/PacienteList.vue'


const pacientes = ref([])

const pacienteObj = reactive({
    id: null,
    nombre: '',
    propietario: '',
    email: '',
    fecha: '',
    sintomas: '', 
  })

  onMounted(()=>{
    leerLS()
  })
  
  const guardarPaciente = ()=>{
    if (pacienteObj.id) {
    const { id } = pacienteObj
    const index = pacientes.value.findIndex(pacienteState => pacienteState.id === id)
    pacientes.value[index] = {...pacienteObj}
  }else{
    pacientes.value.push({...pacienteObj, id: uid()})
  }
  Object.assign(pacienteObj, {
      id: null,
      nombre: '',
      propietario: '',
      email: '',
      fecha: '',
      sintomas: '',
    })
  }
  
  const editarPaciente =(id)=>{
    const pacienteEditar = pacientes.value.filter(paciente => paciente.id === id)[0]
    Object.assign(pacienteObj, pacienteEditar)
  }
  
  const eliminarPaciente = (id)=>{
    pacientes.value = pacientes.value.filter(paciente => paciente.id !== id)
  }
  
  const guardarLS = ()=>{
    localStorage.setItem('pacientes', JSON.stringify(pacientes.value))
  }
  
  const leerLS = ()=>{
    if(localStorage.getItem('pacientes')){
      pacientes.value = JSON.parse(localStorage.getItem('pacientes'))
    }
  }

  watch(pacientes, ()=>{
    guardarLS()
  }, {
    deep: true
  })
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Formulario 
        v-model:nombre="pacienteObj.nombre"
        v-model:propietario="pacienteObj.propietario"
        v-model:email="pacienteObj.email"
        v-model:fecha="pacienteObj.fecha"
        v-model:sintomas="pacienteObj.sintomas"
        @guardar-paciente="guardarPaciente"
        :id="pacienteObj.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h3 class="font-black text-3xl text-center">Administra tus pacientes</h3>
        <div v-if="pacientes.length > 0">
          <p class="text-lg mt-5 text-center mb-10">Información de 
            <span class="text-indigo-600 font-bold">Pacientes</span>
          </p>
            <PacienteList 
              v-for="paciente in pacientes"
              :paciente="paciente"
              @editar-paciente="editarPaciente"
              @eliminar-paciente="eliminarPaciente"
            />
        </div>
        <div v-else>
          <p class="text-center text-2xl mt-5">No hay pacientes registrados</p>
        </div>
      </div>
    </div>
  </div>
  
</template>
