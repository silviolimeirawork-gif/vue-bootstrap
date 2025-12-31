<template>
  <div class="container mt-5">
    <h2>Bootstrap Toasts com Vue 3</h2>

    <!-- Botão para mostrar toast -->
    <button type="button" class="btn btn-primary" @click="showToast">
      Mostrar Toast
    </button>

    <!-- Botão para mostrar toast com template Vue -->
    <button type="button" class="btn btn-success" @click="showCustomToast">
      Toast Personalizado
    </button>

    <!-- Container onde os toasts serão renderizados -->
    <!-- O Bootstrap geralmente anexa os toasts ao body, mas podemos controlar o container -->
    <div ref="toastContainer" class="position-fixed top-0 end-0 p-3" style="z-index: 1055">
      <!-- Toast padrão -->
      <div ref="toastEl" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
        <div class="toast-header">
          <strong class="me-auto">Bootstrap</strong>
          <small>agora</small>
          <button type="button" class="btn-close" data-bs-dismiss="toast" aria-label="Close"></button>
        </div>
        <div class="toast-body">
          Olá, mundo! Este é um toast.
        </div>
      </div>

      <!-- Toast personalizado que será controlado por Vue -->
      <div v-if="showCustomToastFlag" ref="customToastEl" class="toast" role="alert" aria-live="assertive" aria-atomic="true">
        <div class="toast-header">
          <strong class="me-auto">Mensagem Personalizada</strong>
          <button type="button" class="btn-close" @click="hideCustomToast" aria-label="Close"></button>
        </div>
        <div class="toast-body">
          {{ customMessage }}
          <div class="mt-2 pt-2 border-top">
            <button type="button" class="btn btn-primary btn-sm" @click="doSomething">Ação</button>
            <button type="button" class="btn btn-secondary btn-sm" @click="hideCustomToast">Fechar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Toast } from 'bootstrap'

// Referências para os elementos
const toastEl = ref(null)
const customToastEl = ref(null)
const toastContainer = ref(null)

// Estado para o toast personalizado
const showCustomToastFlag = ref(false)
const customMessage = ref('Este é um toast criado com Vue!')

// Instâncias do Bootstrap Toast
let toast = null
let customToast = null

// Inicializar o toast padrão
onMounted(() => {
  if (toastEl.value) {
    toast = new Toast(toastEl.value, {
      autohide: true,
      delay: 3000
    })
  }
})

// Método para mostrar o toast padrão
const showToast = () => {
  if (toast) {
    toast.show()
  }
}

// Método para mostrar o toast personalizado
const showCustomToast = () => {
  showCustomToastFlag.value = true
  // Usar nextTick para garantir que o DOM foi atualizado
  setTimeout(() => {
    if (customToastEl.value) {
      customToast = new Toast(customToastEl.value, {
        autohide: false // vamos controlar manualmente
      })
      customToast.show()
    }
  }, 0)
}

// Método para esconder o toast personalizado
const hideCustomToast = () => {
  if (customToast) {
    customToast.hide()
    // Depois de esconder, podemos remover o elemento do DOM
    // Mas como estamos usando v-if, podemos apenas alterar o flag
    // O evento hidden do Bootstrap pode ser usado para resetar o flag
  }
}

// Configurar evento para quando o toast personalizado for escondido
onMounted(() => {
  if (customToastEl.value) {
    customToastEl.value.addEventListener('hidden.bs.toast', () => {
      showCustomToastFlag.value = false
    })
  }
})

// Limpar eventos e instâncias
onBeforeUnmount(() => {
  if (toast) {
    toast.dispose()
  }
  if (customToast) {
    customToast.dispose()
  }
  if (customToastEl.value) {
    customToastEl.value.removeEventListener('hidden.bs.toast', () => {})
  }
})

// Método de exemplo para a ação no toast
const doSomething = () => {
  alert('Ação realizada!')
  hideCustomToast()
}
</script>

<style scoped>
.container {
  padding: 20px;
}
</style>