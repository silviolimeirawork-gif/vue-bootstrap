<template>
  <div class="container mt-5">
    <h1 class="mb-4">Controle de Opacidade em Vue 3</h1>
    
    <div class="row">
      <!-- Exemplo 1: Opacidade com v-model -->
      <div class="col-md-6 mb-5">
        <h3>1. Controle de Opacidade</h3>
        
        <div class="card">
          <div class="card-body">
            <div class="mb-3">
              <label class="form-label">Opacidade: {{ opacityValue }}%</label>
              <input 
                type="range" 
                class="form-range" 
                min="0" 
                max="100" 
                v-model.number="opacityValue"
              >
            </div>
            
            <div 
              class="opacity-demo-box p-4 rounded"
              :style="{ opacity: opacityValue / 100 }"
            >
              <p class="mb-2">Caixa com opacidade controlada</p>
              <small>Opacity: {{ (opacityValue / 100).toFixed(2) }}</small>
            </div>
            
            <div class="mt-3 d-flex gap-2">
              <button 
                v-for="preset in opacityPresets" 
                :key="preset"
                class="btn btn-sm btn-outline-secondary"
                @click="opacityValue = preset"
              >
                {{ preset }}%
              </button>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Exemplo 2: Fade In/Out -->
      <div class="col-md-6 mb-5">
        <h3>2. Fade In/Out</h3>
        
        <div class="card">
          <div class="card-body">
            <button 
              class="btn btn-primary mb-3"
              @click="toggleFade"
            >
              {{ isVisible ? 'Fade Out' : 'Fade In' }}
            </button>
            
            <transition name="fade">
              <div v-if="isVisible" class="alert alert-success">
                Elemento com transição de opacidade
              </div>
            </transition>
            
            <transition name="fade">
              <div v-if="isVisible" class="mt-3 p-3 bg-light rounded">
                <p>Outro elemento com fade</p>
                <p class="mb-0">Transição suave de opacidade</p>
              </div>
            </transition>
          </div>
        </div>
      </div>
      
      <!-- Exemplo 3: Overlay com opacidade -->
      <div class="col-md-6 mb-5">
        <h3>3. Overlay/Loading</h3>
        
        <div class="card">
          <div class="card-body">
            <button 
              class="btn btn-warning mb-3"
              @click="showOverlay = !showOverlay"
            >
              {{ showOverlay ? 'Ocultar' : 'Mostrar' }} Overlay
            </button>
            
            <div 
              class="position-relative border rounded p-4"
              style="height: 200px;"
            >
              <div class="content">
                <p>Conteúdo principal da área</p>
                <button class="btn btn-sm btn-outline-primary">
                  Botão normal
                </button>
              </div>
              
              <!-- Overlay -->
              <div 
                v-if="showOverlay"
                class="overlay"
                :style="{ opacity: overlayOpacity / 100 }"
              >
                <div class="overlay-content">
                  <div class="spinner-border text-primary" role="status"></div>
                  <p class="mt-2">Carregando...</p>
                </div>
              </div>
            </div>
            
            <div class="mt-3">
              <label class="form-label">Opacidade do Overlay: {{ overlayOpacity }}%</label>
              <input 
                type="range" 
                class="form-range" 
                min="10" 
                max="90" 
                v-model.number="overlayOpacity"
              >
            </div>
          </div>
        </div>
      </div>
      
      <!-- Exemplo 4: Galeria com hover -->
      <div class="col-md-6 mb-5">
        <h3>4. Efeitos Hover</h3>
        
        <div class="card">
          <div class="card-body">
            <div class="row g-3">
              <div 
                v-for="item in galleryItems" 
                :key="item.id"
                class="col-6"
                @mouseenter="item.active = true"
                @mouseleave="item.active = false"
              >
                <div class="gallery-item position-relative">
                  <img 
                    :src="item.image" 
                    :alt="item.title"
                    class="img-fluid rounded"
                    :class="{ 'img-hover': item.active }"
                  >
                  <div 
                    class="gallery-overlay"
                    :class="{ active: item.active }"
                  >
                    <h6 class="text-white mb-1">{{ item.title }}</h6>
                    <p class="text-white small mb-0">{{ item.description }}</p>
                  </div>
                </div>
              </div>
            </div>
            
            <div class="mt-3">
              <div class="form-check form-switch">
                <input 
                  class="form-check-input" 
                  type="checkbox" 
                  v-model="enableHover"
                  id="hoverSwitch"
                >
                <label class="form-check-label" for="hoverSwitch">
                  Ativar efeitos hover
                </label>
              </div>
            </div>
          </div>
        </div>
      </div>
      
      <!-- Exemplo 5: Opacidade condicional -->
      <div class="col-md-12 mb-5">
        <h3>5. Opacidade Condicional</h3>
        
        <div class="card">
          <div class="card-body">
            <div class="row align-items-center mb-4">
              <div class="col-md-4">
                <div class="mb-3">
                  <label class="form-label">Nível de prioridade:</label>
                  <select v-model="priorityLevel" class="form-select">
                    <option value="high">Alta</option>
                    <option value="medium">Média</option>
                    <option value="low">Baixa</option>
                  </select>
                </div>
              </div>
              
              <div class="col-md-8">
                <div class="priority-list">
                  <div 
                    v-for="task in tasks" 
                    :key="task.id"
                    class="priority-item p-3 mb-2 rounded"
                    :class="[
                      `priority-${task.priority}`,
                      { 'current-priority': task.priority === priorityLevel }
                    ]"
                    :style="{
                      opacity: getTaskOpacity(task.priority)
                    }"
                  >
                    <div class="d-flex justify-content-between align-items-center">
                      <span>{{ task.name }}</span>
                      <span class="badge" :class="`bg-${getPriorityColor(task.priority)}`">
                        {{ task.priority === 'high' ? 'Alta' : task.priority === 'medium' ? 'Média' : 'Baixa' }}
                      </span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            
            <div class="alert alert-info">
              <p class="mb-0">Tarefas com prioridade diferente da selecionada têm opacidade reduzida.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Exemplo 1
const opacityValue = ref(70)
const opacityPresets = [20, 40, 60, 80, 100]

// Exemplo 2
const isVisible = ref(true)
const toggleFade = () => {
  isVisible.value = !isVisible.value
}

// Exemplo 3
const showOverlay = ref(false)
const overlayOpacity = ref(70)

// Exemplo 4
const enableHover = ref(true)
const galleryItems = ref([
  {
    id: 1,
    title: 'Imagem 1',
    description: 'Descrição da imagem 1',
    image: 'https://picsum.photos/300/200?random=1',
    active: false
  },
  {
    id: 2,
    title: 'Imagem 2',
    description: 'Descrição da imagem 2',
    image: 'https://picsum.photos/300/200?random=2',
    active: false
  },
  {
    id: 3,
    title: 'Imagem 3',
    description: 'Descrição da imagem 3',
    image: 'https://picsum.photos/300/200?random=3',
    active: false
  },
  {
    id: 4,
    title: 'Imagem 4',
    description: 'Descrição da imagem 4',
    image: 'https://picsum.photos/300/200?random=4',
    active: false
  }
])

// Exemplo 5
const priorityLevel = ref('high')
const tasks = ref([
  { id: 1, name: 'Tarefa Urgente', priority: 'high' },
  { id: 2, name: 'Reunião com equipe', priority: 'high' },
  { id: 3, name: 'Documentação do projeto', priority: 'medium' },
  { id: 4, name: 'Atualizar dependências', priority: 'medium' },
  { id: 5, name: 'Organizar arquivos', priority: 'low' },
  { id: 6, name: 'Ler artigos', priority: 'low' }
])

const getTaskOpacity = (taskPriority) => {
  if (priorityLevel.value === taskPriority) return 1
  if (priorityLevel.value === 'high' && taskPriority === 'medium') return 0.6
  if (priorityLevel.value === 'high' && taskPriority === 'low') return 0.3
  if (priorityLevel.value === 'medium' && taskPriority === 'high') return 0.8
  if (priorityLevel.value === 'medium' && taskPriority === 'low') return 0.4
  if (priorityLevel.value === 'low' && taskPriority === 'high') return 0.7
  if (priorityLevel.value === 'low' && taskPriority === 'medium') return 0.5
  return 1
}

const getPriorityColor = (priority) => {
  return {
    high: 'danger',
    medium: 'warning',
    low: 'success'
  }[priority]
}
</script>

<style scoped>
/* Estilos para o Exemplo 1 */
.opacity-demo-box {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  transition: opacity 0.3s ease;
  height: 150px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/* Estilos para o Exemplo 2 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Estilos para o Exemplo 3 */
.position-relative {
  position: relative;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  transition: opacity 0.3s ease;
}

.overlay-content {
  text-align: center;
  color: white;
}

.content {
  z-index: 1;
  position: relative;
}

/* Estilos para o Exemplo 4 */
.gallery-item {
  cursor: pointer;
  overflow: hidden;
  border-radius: 8px;
}

.gallery-item img {
  transition: all 0.3s ease;
  width: 100%;
  height: 150px;
  object-fit: cover;
}

.gallery-item:hover img {
  transform: scale(1.05);
}

.img-hover {
  opacity: 0.7;
}

.gallery-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 10px;
  transform: translateY(100%);
  transition: transform 0.3s ease, opacity 0.3s ease;
  opacity: 0;
}

.gallery-overlay.active {
  transform: translateY(0);
  opacity: 1;
}

/* Estilos para o Exemplo 5 */
.priority-list {
  min-height: 200px;
}

.priority-item {
  transition: all 0.3s ease;
  cursor: pointer;
}

.priority-item:hover {
  transform: translateX(5px);
}

.priority-high {
  background-color: rgba(220, 53, 69, 0.1);
  border-left: 4px solid #dc3545;
}

.priority-medium {
  background-color: rgba(255, 193, 7, 0.1);
  border-left: 4px solid #ffc107;
}

.priority-low {
  background-color: rgba(25, 135, 84, 0.1);
  border-left: 4px solid #198754;
}

.current-priority {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

/* Estilos gerais */
.card {
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-2px);
}

.form-range::-webkit-slider-thumb {
  background-color: #0d6efd;
}

.form-range::-moz-range-thumb {
  background-color: #0d6efd;
}
</style>
