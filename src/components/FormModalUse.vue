<template>
  <button @click="showFormModal = true" class="btn btn-primary">
    Abrir Form Modal
  </button>

  <FormModal
    v-model:visible="showFormModal"
    title="Cadastro de Usuário"
    :initial-data="userData"
    @submit="handleSubmit"
  >
    <template #default="{ formData, errors }">
      <div class="mb-3">
        <label class="form-label">Nome</label>
        <input 
          v-model="formData.name"
          type="text" 
          class="form-control"
          :class="{ 'is-invalid': errors.name }"
        >
        <div v-if="errors.name" class="invalid-feedback">
          {{ errors.name[0] }}
        </div>
      </div>
      
      <div class="mb-3">
        <label class="form-label">Email</label>
        <input 
          v-model="formData.email"
          type="email" 
          class="form-control"
          :class="{ 'is-invalid': errors.email }"
        >
        <div v-if="errors.email" class="invalid-feedback">
          {{ errors.email[0] }}
        </div>
      </div>
    </template>
  </FormModal>
</template>

<script setup>
import { ref } from 'vue'
import FormModal from './FormModal.vue'

const showFormModal = ref(false)
const userData = ref({
  name: '',
  email: ''
})

const handleSubmit = async (formData) => {
  // Simular API call
  console.log('Dados enviados:', formData)
  
  // Simular erro
  // throw new Error('Erro de validação')
}
</script>
