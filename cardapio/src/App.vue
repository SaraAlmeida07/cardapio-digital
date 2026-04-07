<script setup>
import { ref, computed } from 'vue'

import CabecalhoApp from './components/CabecalhoApp.vue'
import FormularioItem from './components/FormularioItem.vue'  

const paginaAtual = ref('vitrine')
</script>

<template>

  <CabecalhoApp 
      :paginaAtual="paginaAtual" 
      @mudar-pagina="(novaPagina) => paginaAtual = novaPagina" 
    />

  <div class="container">
    <main v-if="paginaAtual === 'cadastro'">
      <h2>Cadastrar Novo Produto</h2>
    </main>

    <main v-else-if="paginaAtual === 'vitrine'">
      <h2>Vitrine de Produtos</h2>
    </main>

  </div>
</template>

<style scoped>
/* Apenas um estilo básico para centralizar o conteúdo da tela */
.container {
  max-width: 1100px;
  margin: 0 auto;
  padding: 20px;
}
</style>