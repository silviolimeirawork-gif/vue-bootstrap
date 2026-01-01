

<template>
  <div class="container mt-5">
    <h1 class="mb-4">Vue 3 + Bootstrap Tooltips</h1>
    
    <div class="row">
      <!-- Tooltip básico -->
      <div class="col-md-4 mb-3">
        <button 
          type="button" 
          class="btn btn-primary"
          data-bs-toggle="tooltip"
          data-bs-placement="top"
          :title="tooltipText"
          ref="tooltip1"
        >
          Tooltip no topo
        </button>
      </div>

      <!-- Tooltip com conteúdo dinâmico -->
      <div class="col-md-4 mb-3">
        <button 
          type="button" 
          class="btn btn-success"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          :data-bs-title="dynamicTooltip"
          ref="tooltip2"
        >
          Tooltip dinâmico
        </button>
        <div class="mt-2">
          <input 
            v-model="dynamicTooltip" 
            class="form-control" 
            placeholder="Digite o texto do tooltip"
          />
        </div>
      </div>

      <!-- Tooltip customizado -->
      <div class="col-md-4 mb-3">
        <button 
          type="button" 
          class="btn btn-warning"
          data-bs-toggle="tooltip"
          data-bs-placement="bottom"
          data-bs-custom-class="custom-tooltip"
          data-bs-title="Este é um tooltip customizado!"
          ref="tooltip3"
        >
          Tooltip customizado
        </button>
      </div>

      <!-- Tooltip em elementos não-button -->
      <div class="col-md-4 mb-3">
        <span 
          class="badge bg-info"
          data-bs-toggle="tooltip"
          data-bs-placement="left"
          data-bs-title="Tooltip em um span"
          ref="tooltip4"
        >
          Passe o mouse aqui
        </span>
      </div>

      <!-- Tooltip programático -->
      <div class="col-md-4 mb-3">
        <button 
          type="button" 
          class="btn btn-danger"
          ref="programmaticTooltip"
          @click="toggleTooltip"
        >
          {{ isTooltipVisible ? 'Ocultar' : 'Mostrar' }} Tooltip
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
//exemplo vue 3 vite bootstrap tooltip
  
// CORREÇÃO: Importar watch do Vue
import { ref, onMounted, onUnmounted, nextTick, watch } from 'vue'
import { Tooltip } from 'bootstrap'

// Refs para os elementos
const tooltip1 = ref(null)
const tooltip2 = ref(null)
const tooltip3 = ref(null)
const tooltip4 = ref(null)
const programmaticTooltip = ref(null)

// Dados reativos
const tooltipText = ref('Este é um tooltip básico!')
const dynamicTooltip = ref('Texto dinâmico do tooltip')
const isTooltipVisible = ref(false)

// Instâncias dos tooltips
let tooltipInstances = []

// Inicializar tooltips quando o componente montar
onMounted(() => {
  // Inicializar tooltips com data-bs-toggle
  initializeTooltips()
  
  // Tooltip programático
  const programmaticInstance = new Tooltip(programmaticTooltip.value, {
    title: 'Tooltip controlado programaticamente',
    placement: 'bottom'
  })
  tooltipInstances.push(programmaticInstance)
})

// Limpar tooltips quando o componente desmontar
onUnmounted(() => {
  tooltipInstances.forEach(instance => {
    if (instance && instance.dispose) {
      instance.dispose()
    }
  })
})

// Inicializar todos os tooltips
const initializeTooltips = () => {
  // Usar nextTick para garantir que o DOM esteja renderizado
  nextTick(() => {
    // Selecionar todos os elementos com data-bs-toggle="tooltip"
    const tooltipElements = [tooltip1, tooltip2, tooltip3, tooltip4]
    
    tooltipElements.forEach(element => {
      if (element.value) {
        const tooltip = new Tooltip(element.value)
        tooltipInstances.push(tooltip)
      }
    })
  })
}

// Controlar tooltip programaticamente
const toggleTooltip = () => {
  const instance = tooltipInstances.find(
    inst => inst._element === programmaticTooltip.value
  )
  
  if (instance) {
    if (isTooltipVisible.value) {
      instance.hide()
    } else {
      instance.show()
    }
    isTooltipVisible.value = !isTooltipVisible.value
  }
}

// CORREÇÃO: Watch para atualizar tooltip dinâmico
watch(dynamicTooltip, (newValue) => {
  const instance = tooltipInstances.find(
    inst => inst._element === tooltip2.value
  )
  
  if (instance) {
    // Método correto para atualizar conteúdo no Bootstrap 5
    instance.setContent({ '.tooltip-inner': newValue })
    
    // Se o tooltip estiver visível, atualizar imediatamente
    if (instance.tip && instance.tip.classList.contains('show')) {
      instance.update()
    }
  }
})
</script>

<style>
/* Estilos customizados para tooltips */
.custom-tooltip .tooltip-inner {
  background-color: #ff9900;
  color: #000;
  font-weight: bold;
}

.custom-tooltip .tooltip-arrow {
  border-top-color: #ff9900 !important;
}

.container {
  max-width: 900px;
}
</style>
