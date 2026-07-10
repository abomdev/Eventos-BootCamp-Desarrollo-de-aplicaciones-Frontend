<template>
  <main class="container">
    <header class="header">
      <h1>Centro de Eventos</h1>
      <p class="subtitle">
        Modificadores de eventos, modificadores de teclado y propagación de eventos en Vue 3.
      </p>
    </header>

    <div class="layout">
      <div class="modules">
        <section class="card">
          <h2 class="card-title">1. Modificadores de eventos</h2>

          <div class="demo-row">
            <button class="btn btn-primary" @click.once="onOnceClick">
              {{ onceUsado ? 'Ya no reacciona (haz clic de nuevo)' : 'Presionar (.once)' }}
            </button>
          </div>

          <div class="demo-row self-box" @click.self="onSelfClick">
            <p class="self-hint">Haz clic en el área vacía (fuera del botón) para disparar el log (.self)</p>
            <button class="btn btn-secondary" @click="onInnerButtonClick">
              Botón interior (no dispara .self)
            </button>
          </div>

          <div class="demo-row">
            <a href="#eventos" class="link-demo" @click.prevent="onPreventClick">
              Este link no navega (.prevent)
            </a>
          </div>

          <form class="demo-row mini-form" @submit.prevent="onSubmitDemo">
            <input v-model="textoForm" type="text" placeholder="Escribe algo y envía" />
            <button type="submit" class="btn btn-secondary">Enviar (.prevent en submit)</button>
          </form>
        </section>

        <section class="card">
          <h2 class="card-title">2. Modificadores de teclado</h2>
          <input
            v-model="textoTeclado"
            type="text"
            class="keyboard-input"
            placeholder="Prueba Enter, Esc, Delete, Ctrl + Enter..."
            @keyup.enter="onKeyEnter"
            @keyup.esc="onKeyEsc"
            @keyup.delete="onKeyDelete"
            @keyup.ctrl.enter="onKeyCtrlEnter"
          />
          <p class="hint">
            Prueba: <code>Enter</code>, <code>Esc</code>, <code>Delete</code>/<code>Backspace</code> y
            <code>Ctrl + Enter</code>.
          </p>
        </section>

        <section class="card">
          <h2 class="card-title">3. Propagación de eventos</h2>

          <label class="checkbox-label">
            <input v-model="detenerPropagacion" type="checkbox" />
            Detener propagación en la capa Interior (stopPropagation)
          </label>

          <div class="prop-box prop-exterior" @click="registrarEvento('Capa Exterior')">
            Exterior
            <div class="prop-box prop-medio" @click="registrarEvento('Capa Medio')">
              Medio
              <div class="prop-box prop-interior" @click="onInnerPropClick">Interior (haz clic aquí)</div>
            </div>
          </div>
        </section>
      </div>

      <aside class="card log-panel">
        <div class="log-header">
          <h2 class="card-title">Registro de eventos</h2>
          <button class="btn btn-secondary" @click="limpiarRegistro">Limpiar</button>
        </div>

        <p v-if="registro.length === 0" class="log-empty">
          Todavía no se registró ningún evento. Prueba los módulos de la izquierda.
        </p>

        <ul v-else class="log-list">
          <li v-for="entrada in registro" :key="entrada.id">
            <span class="log-time">{{ entrada.hora }}</span>
            <span class="log-message">{{ entrada.mensaje }}</span>
          </li>
        </ul>
      </aside>
    </div>
  </main>
</template>

<script setup>
import { ref } from 'vue'

const registro = ref([])

function registrarEvento(mensaje) {
  const ahora = new Date()
  const hora =
    ahora.toLocaleTimeString('es-CL', { hour12: false }) +
    '.' +
    String(ahora.getMilliseconds()).padStart(3, '0')

  registro.value.unshift({ id: crypto.randomUUID(), hora, mensaje })
}

function limpiarRegistro() {
  registro.value = []
}

// Módulo 1: modificadores de eventos
const onceUsado = ref(false)

function onOnceClick() {
  onceUsado.value = true
  registrarEvento('Botón .once presionado - esta es la única vez que se va a registrar')
}

function onSelfClick() {
  registrarEvento('Click en el contenedor (.self) - el target fue el propio contenedor')
}

function onInnerButtonClick() {
  registrarEvento('Click en el botón interior - no dispara el handler .self del contenedor padre')
}

function onPreventClick() {
  registrarEvento('Click en el link - se evitó la navegación con .prevent')
}

const textoForm = ref('')

function onSubmitDemo() {
  registrarEvento(`Submit del formulario evitado con .prevent (texto: "${textoForm.value || 'vacío'}")`)
}

// Módulo 2: modificadores de teclado
const textoTeclado = ref('')

function onKeyEnter() {
  registrarEvento('Tecla Enter detectada (@keyup.enter)')
}

function onKeyEsc() {
  registrarEvento('Tecla Escape detectada (@keyup.esc)')
}

function onKeyDelete() {
  registrarEvento('Tecla Delete/Backspace detectada (@keyup.delete)')
}

function onKeyCtrlEnter() {
  registrarEvento('Combinación Ctrl + Enter detectada (@keyup.ctrl.enter)')
}

// Módulo 3: propagación de eventos
const detenerPropagacion = ref(false)

function onInnerPropClick(event) {
  if (detenerPropagacion.value) {
    event.stopPropagation()
    registrarEvento('Capa Interior - propagación detenida con stopPropagation()')
  } else {
    registrarEvento('Capa Interior - el click se sigue propagando hacia afuera')
  }
}
</script>
