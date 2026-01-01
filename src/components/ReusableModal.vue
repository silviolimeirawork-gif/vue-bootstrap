<template>
  <transition name="modal">
    <div 
      v-if="modelValue" 
      class="modal-overlay"
      @click.self="closeModal"
    >
      <div 
        class="modal-container"
        :class="sizeClass"
      >
        <div class="modal-content">
          <!-- Cabeçalho -->
          <div class="modal-header">
            <h5 class="modal-title">
              <slot name="title">
                {{ title }}
              </slot>
            </h5>
            <button 
              type="button" 
              class="btn-close" 
              @click="closeModal"
              aria-label="Close"
            ></button>
          </div>
          
          <!-- Corpo -->
          <div class="modal-body">
            <slot></slot>
          </div>
          
          <!-- Rodapé -->
          <div class="modal-footer">
            <slot name="footer">
              <button 
                type="button" 
                class="btn btn-secondary" 
                @click="cancel"
              >
                Cancelar
              </button>
              <button 
                type="button" 
                class="btn btn-primary" 
                @click="confirm"
              >
                Confirmar
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  modelValue: {  // CORRIGIDO: usando modelValue (padrão do v-model)
    type: Boolean,
    required: true
  },
  title: {
    type: String,
    default: 'Modal'
  },
  size: {
    type: String,
    default: 'md',
    validator: (value) => ['sm', 'md', 'lg', 'xl'].includes(value)
  },
  closeOnBackdrop: {
    type: Boolean,
    default: true
  }
})

const emit = defineEmits([
  'update:modelValue',  // CORRIGIDO: emit padrão do v-model
  'confirm',
  'cancel'
])

const sizeClass = computed(() => {
  return {
    'sm': 'modal-sm',
    'md': '',
    'lg': 'modal-lg',
    'xl': 'modal-xl'
  }[props.size]
})

const closeModal = () => {
  if (props.closeOnBackdrop) {
    emit('update:modelValue', false)
  }
}

const confirm = () => {
  emit('confirm')
  emit('update:modelValue', false)
}

const cancel = () => {
  emit('cancel')
  emit('update:modelValue', false)
}

// Fechar com ESC
const handleKeydown = (e) => {
  if (e.key === 'Escape' && props.modelValue) {
    closeModal()
  }
}

// Adicionar/remover event listener
if (typeof window !== 'undefined') {
  window.addEventListener('keydown', handleKeydown)
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1050;
  padding: 20px;
}

.modal-container {
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
  width: 100%;
  max-height: 90vh;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.modal-container.modal-sm {
  max-width: 400px;
}

.modal-container {
  max-width: 500px;
}

.modal-container.modal-lg {
  max-width: 800px;
}

.modal-container.modal-xl {
  max-width: 1140px;
}

.modal-content {
  display: flex;
  flex-direction: column;
  height: 100%;
}

.modal-header {
  padding: 1rem 1.5rem;
  border-bottom: 1px solid #dee2e6;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.modal-title {
  margin: 0;
  font-size: 1.25rem;
}

.modal-body {
  padding: 1.5rem;
  flex: 1;
  overflow-y: auto;
}

.modal-footer {
  padding: 1rem 1.5rem;
  border-top: 1px solid #dee2e6;
  display: flex;
  justify-content: flex-end;
  gap: 0.5rem;
}

/* Transições */
.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

.modal-enter-active .modal-container,
.modal-leave-active .modal-container {
  transition: transform 0.3s ease;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  transform: scale(0.95);
}
</style>
