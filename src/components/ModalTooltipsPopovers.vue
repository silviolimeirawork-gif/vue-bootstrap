<template>
  <div class="container mt-5">
    <h1 class="mb-4">Vue 3 + Bootstrap Modals</h1>

    <!-- Modal simples com data attributes -->
    <div class="mb-3">
      <h3>1. Modal com data-bs-*</h3>
      <button 
        type="button" 
        class="btn btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#simpleModal"
      >
        Abrir Modal Simples
      </button>
    </div>

    <!-- Modal Simples -->
    <div class="modal fade" id="simpleModal" tabindex="-1" aria-labelledby="simpleModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="simpleModalLabel">Modal Simples</h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <p>Este é um modal usando apenas data-bs-* attributes.</p>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Fechar</button>
            <button type="button" class="btn btn-primary">Salvar</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal controlado por Vue -->
    <div class="mb-3">
      <h3>2. Modal Controlado por Vue</h3>
      <button 
        type="button" 
        class="btn btn-success"
        @click="openModal('vueModal')"
      >
        Abrir Modal Vue
      </button>
    </div>

    <!-- Modal Vue -->
    <div 
      class="modal fade" 
      id="vueModal" 
      tabindex="-1" 
      aria-hidden="true"
      ref="vueModalRef"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ modalTitle }}</h5>
            <button 
              type="button" 
              class="btn-close" 
              @click="closeModal('vueModal')"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <div class="mb-3">
              <label for="nome" class="form-label">Nome</label>
              <input 
                type="text" 
                class="form-control" 
                id="nome" 
                v-model="formData.nome"
                placeholder="Digite seu nome"
              >
            </div>
            <div class="mb-3">
              <label for="email" class="form-label">Email</label>
              <input 
                type="email" 
                class="form-control" 
                id="email" 
                v-model="formData.email"
                placeholder="Digite seu email"
              >
            </div>
            <p v-if="modalMessage" class="text-success">{{ modalMessage }}</p>
          </div>
          <div class="modal-footer">
            <button 
              type="button" 
              class="btn btn-secondary" 
              @click="closeModal('vueModal')"
            >
              Cancelar
            </button>
            <button 
              type="button" 
              class="btn btn-primary" 
              @click="submitForm"
            >
              Salvar
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal Dinâmico -->
    <div class="mb-3">
      <h3>3. Modal Dinâmico</h3>
      <button 
        type="button" 
        class="btn btn-warning"
        @click="showDynamicModal = true"
      >
        Abrir Modal Dinâmico
      </button>
    </div>

    <!-- Modal Dinâmico com v-if -->
    <div 
      v-if="showDynamicModal" 
      class="modal fade show d-block" 
      tabindex="-1"
      style="background-color: rgba(0,0,0,0.5)"
      @click.self="closeDynamicModal"
    >
      <div class="modal-dialog modal-dialog-centered">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Modal Dinâmico</h5>
            <button 
              type="button" 
              class="btn-close" 
              @click="closeDynamicModal"
            ></button>
          </div>
          <div class="modal-body">
            <p>Conteúdo dinâmico do modal. Status: <strong>{{ dynamicStatus }}</strong></p>
            <button 
              class="btn btn-sm btn-info"
              @click="toggleStatus"
            >
              Alternar Status
            </button>
          </div>
          <div class="modal-footer">
            <button 
              type="button" 
              class="btn btn-secondary" 
              @click="closeDynamicModal"
            >
              Fechar
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal com slots (Componente reutilizável) -->
    <div class="mb-3">
      <h3>4. Modal Componente Reutilizável</h3>
      <button 
        type="button" 
        class="btn btn-info"
        @click="showReusableModal = true"
      >
        Abrir Modal Reutilizável
      </button>
    </div>

    <!-- Componente Modal Reutilizável - CORRIGIDO -->
    <ReusableModal 
      v-model="showReusableModal"
      :title="reusableModalTitle"
      @confirm="handleConfirm"
      @cancel="handleCancel"
    >
      <template #default>
        <p>Conteúdo personalizado usando slots.</p>
        <div class="alert alert-info">
          Este conteúdo é passado através do slot default.
        </div>
      </template>
      
      <template #footer>
        <button type="button" class="btn btn-outline-secondary" @click="handleCancel">
          Cancelar
        </button>
        <button type="button" class="btn btn-danger" @click="handleConfirm">
          Confirmar Ação
        </button>
      </template>
    </ReusableModal>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Modal } from 'bootstrap'
import ReusableModal from './ReusableModal.vue'

// Refs para modais
const vueModalRef = ref(null)
const showDynamicModal = ref(false)
const showReusableModal = ref(false)

// Dados do formulário
const modalTitle = ref('Formulário Vue Modal')
const formData = ref({
  nome: '',
  email: ''
})
const modalMessage = ref('')
const dynamicStatus = ref('Ativo')
const reusableModalTitle = ref('Modal Reutilizável')

// Instância do modal Bootstrap
let vueModalInstance = null

// Inicializar modal quando componente montar
onMounted(() => {
  if (vueModalRef.value) {
    vueModalInstance = new Modal(vueModalRef.value, {
      backdrop: 'static',
      keyboard: false
    })
  }
})

// Limpar quando desmontar
onUnmounted(() => {
  if (vueModalInstance) {
    vueModalInstance.dispose()
  }
})

// Métodos para modal Vue
const openModal = (modalId) => {
  if (modalId === 'vueModal' && vueModalInstance) {
    modalMessage.value = ''
    vueModalInstance.show()
  }
}

const closeModal = (modalId) => {
  if (modalId === 'vueModal' && vueModalInstance) {
    vueModalInstance.hide()
  }
}

const submitForm = () => {
  if (!formData.value.nome || !formData.value.email) {
    modalMessage.value = 'Por favor, preencha todos os campos!'
    return
  }
  
  modalMessage.value = `Dados salvos: ${formData.value.nome} (${formData.value.email})`
  
  // Simular API call
  setTimeout(() => {
    closeModal('vueModal')
    formData.value = { nome: '', email: '' }
  }, 1500)
}

// Métodos para modal dinâmico
const closeDynamicModal = () => {
  showDynamicModal.value = false
}

const toggleStatus = () => {
  dynamicStatus.value = dynamicStatus.value === 'Ativo' ? 'Inativo' : 'Ativo'
}

// Métodos para modal reutilizável
const handleConfirm = () => {
  alert('Ação confirmada!')
  showReusableModal.value = false
}

const handleCancel = () => {
  showReusableModal.value = false
}
</script>

<style scoped>
.modal-backdrop {
  opacity: 0.5;
}

.container {
  max-width: 1200px;
}

.modal-content {
  border-radius: 10px;
}

.modal-header {
  background-color: #f8f9fa;
  border-radius: 10px 10px 0 0;
}
</style>
