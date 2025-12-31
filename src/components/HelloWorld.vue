<!-- <script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})

const count = ref(0)
</script> -->

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { Popover } from 'bootstrap'

// Refs para os elementos
const popoverButton1 = ref(null)
const popoverButton2 = ref(null)
const popoverButton3 = ref(null)
const popoverButton4 = ref(null)
const titlePopover = ref(null)

// Instâncias dos popovers
let popover1 = null
let popover2 = null
let popover3 = null
let popover4 = null
let titlePopoverInstance = null

// Conteúdo HTML para o popover
const htmlContent = `
  <div>
    <strong>Conteúdo HTML</strong>
    <ul>
      <li>Item 1</li>
      <li>Item 2</li>
      <li>Item 3</li>
    </ul>
  </div>
`

// Inicializar popovers quando o componente for montado
onMounted(() => {
  // Popovers com data attributes
  if (popoverButton1.value) {
    popover1 = new Popover(popoverButton1.value)
  }
  
  if (popoverButton2.value) {
    popover2 = new Popover(popoverButton2.value)
  }
  
  if (popoverButton3.value) {
    popover3 = new Popover(popoverButton3.value)
  }
  
  // Popover programático
  if (popoverButton4.value) {
    popover4 = new Popover(popoverButton4.value, {
      title: 'Popover Programático',
      content: 'Este popover foi criado via JavaScript.',
      placement: 'bottom',
      trigger: 'manual'
    })
  }
  
  // Popover em elemento customizado (hover)
  if (titlePopover.value) {
    titlePopoverInstance = new Popover(titlePopover.value, {
      title: 'Informação',
      content: 'Este é um card de exemplo.',
      placement: 'top',
      trigger: 'hover'
    })
  }
})

// Método para alternar popover programático
const toggleCustomPopover = () => {
  if (popover4) {
    if (popoverButton4.value.classList.contains('show')) {
      popover4.hide()
    } else {
      popover4.show()
    }
  }
}

// Limpar instâncias ao desmontar
onBeforeUnmount(() => {
  if (popover1) popover1.dispose()
  if (popover2) popover2.dispose()
  if (popover3) popover3.dispose()
  if (popover4) popover4.dispose()
  if (titlePopoverInstance) titlePopoverInstance.dispose()
})
</script>



<template>
  <h1>{{ msg }}</h1>

  <div class="card">
    <button type="button" @click="count++">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Learn more about IDE Support for Vue in the
    <a
      href="https://vuejs.org/guide/scaling-up/tooling.html#ide-support"
      target="_blank"
      >Vue Docs Scaling up Guide</a
    >.
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>

  <div class="container mt-5">
    <h1 class="text-primary">Hello, Vite + Vue 3 + Bootstrap!</h1>
    <button type="button" class="btn btn-primary">Primary Button</button>

    <!-- Example of a Bootstrap JS component: a simple alert -->
    <div class="alert alert-success mt-3" role="alert">
      This is a success alert—check it out!
    </div>
    <div class="collapse mt-2" id="collapseExample">
      <div class="card card-body">
        This is some placeholder content for a collapse component.
      </div>
    </div>  

    <hr>
    <hr>
 <button
      ref="popoverButton1"
      type="button"
      class="btn btn-primary m-2"
      data-bs-toggle="popover"
      data-bs-title="Título do Popover"
      data-bs-content="Conteúdo do popover aqui."
    >
      Popover básico
    </button>

    <!-- Exemplo 2: Popover com HTML -->
    <button
      ref="popoverButton2"
      type="button"
      class="btn btn-success m-2"
      data-bs-toggle="popover"
      data-bs-title="Popover com HTML"
      :data-bs-content="htmlContent"
      data-bs-html="true"
    >
      Popover com HTML
    </button>

    <!-- Exemplo 3: Popover com posicionamento -->
    <button
      ref="popoverButton3"
      type="button"
      class="btn btn-warning m-2"
      data-bs-toggle="popover"
      data-bs-title="Popover à direita"
      data-bs-content="Aparece à direita do elemento"
      data-bs-placement="right"
    >
      Popover à direita
    </button>

    <!-- Exemplo 4: Popover programático -->
    <button
      ref="popoverButton4"
      type="button"
      class="btn btn-info m-2"
      @click="toggleCustomPopover"
    >
      Popover programático
    </button>

    <!-- Exemplo 5: Popover em elemento customizado -->
    <div class="card mt-4" style="width: 18rem;">
      <div class="card-body">
        <h5 class="card-title" 
          ref="titlePopover">
          Card Title</h5>
        <p class="card-text">Passe o mouse sobre o título para ver o popover.</p>
      </div>
    </div>    
    <br>
    <br>
    <br>
    <br>

  </div>  
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>

<style scoped>
.container {
  padding: 20px;
}
.btn {
  margin: 5px;
}
</style>