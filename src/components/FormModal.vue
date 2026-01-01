<template>
  <div 
    v-if="visible" 
    class="modal-backdrop fade show"
    @click.self="onBackdropClick"
  >
    <div class="modal fade show d-block" tabindex="-1">
      <div class="modal-dialog" :class="modalSize">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">{{ title }}</h5>
            <button 
              type="button" 
              class="btn-close" 
              @click="close"
              aria-label="Close"
            ></button>
          </div>
          
          <form @submit.prevent="submit">
            <div class="modal-body">
              <slot :form-data="formData" :errors="errors"></slot>
            </div>
            
            <div class="modal-footer">
              <slot name="footer" :close="close" :submit="submit">
                <button 
                  type="button" 
                  class="btn btn-secondary" 
                  @click="close"
                >
                  Cancelar
                </button>
                <button 
                  type="submit" 
                  class="btn btn-primary" 
                  :disabled="loading"
                >
                  <span v-if="loading" class="spinner-border spinner-border-sm me-1"></span>
                  {{ submitText }}
                </button>
              </slot>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  visible: {
    type: Boolean,
    required: true
  },
  title: {
    type: String,
    default: 'Formulário'
  },
  size: {
    type: String,
    default: '',
    validator: (value) => ['', 'modal-sm', 'modal-lg', 'modal-xl'].includes(value)
  },
  submitText: {
    type: String,
    default: 'Salvar'
  },
  closeOnBackdrop: {
    type: Boolean,
    default: true
  },
  initialData: {
    type: Object,
    default: () => ({})
  }
})

const emit = defineEmits([
  'update:visible',
  'submit',
  'close'
])

const loading = ref(false)
const formData = ref({ ...props.initialData })
const errors = ref({})

const modalSize = props.size

watch(() => props.visible, (newVal) => {
  if (newVal) {
    formData.value = { ...props.initialData }
    errors.value = {}
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
  }
})

const close = () => {
  emit('update:visible', false)
  emit('close')
}

const onBackdropClick = () => {
  if (props.closeOnBackdrop) {
    close()
  }
}

const submit = async () => {
  loading.value = true
  errors.value = {}
  
  try {
    await emit('submit', formData.value)
    close()
  } catch (error) {
    if (error.response?.data?.errors) {
      errors.value = error.response.data.errors
    }
  } finally {
    loading.value = false
  }
}

// Fechar com ESC
const handleEscape = (e) => {
  if (e.key === 'Escape' && props.visible) {
    close()
  }
}

// Adicionar event listeners
if (typeof window !== 'undefined') {
  window.addEventListener('keydown', handleEscape)
}
</script>

<style scoped>
.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1040;
}

.modal {
  z-index: 1050;
}

.modal-content {
  animation: modalShow 0.3s ease-out;
}

@keyframes modalShow {
  from {
    opacity: 0;
    transform: translateY(-50px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
