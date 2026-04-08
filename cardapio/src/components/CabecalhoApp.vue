<script setup>
// Usamos defineProps para receber do App.vue qual é a página atual
defineProps({
  paginaAtual: String
})

// Usamos defineEmits para avisar o App.vue quando o usuário clicar em um botão
const emit = defineEmits(['mudar-pagina'])
</script>

<template>
  <header class="cabecalho-fundo">
    <div class="cabecalho-conteudo">
      <div class="logo-titulo">
        <span class="icone">🍔</span>
        <h1>Cardápio Digital</h1>
      </div>
    </div>
  </header>
</template>

<style scoped>
.cabecalho-fundo {
  background-color: var(--cor-fundo-card);
  border-bottom: 1px solid var(--borda-suave);
  border-radius: 10px;
  width: 100%;
}

.cabecalho-conteudo {
  max-width: 1200px; /* Aumentei um pouco para caber o layout lado a lado */
  margin: 0 auto;
  padding: 10px 10px;
  display: flex;
  align-items: center; 
}

.logo-titulo {
  display: flex;
  align-items: center;
  gap: 10px;
}

.icone {
  font-size: 2rem;
}

h1 {
  font-size: 1.5rem;
  color: var(--cor-texto-principal);
}
</style>