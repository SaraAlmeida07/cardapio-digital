<script setup>
// Usamos defineProps para receber do App.vue qual é a página atual
defineProps({
  paginaAtual: String
})

// Usamos defineEmits para avisar o App.vue quando o usuário clicar em um botão
const emit = defineEmits(['mudar-pagina'])
</script>

<template>
  <header class="cabecalho">
    <div class="logo-titulo">
      <span class="icone">🍔</span>
      <h1>Cardápio Digital</h1>
    </div>
    
    <nav class="menu-navegacao">
      <button 
        :class="{ ativo: paginaAtual === 'vitrine' }" 
        @click="emit('mudar-pagina', 'vitrine')"
      >
        Vitrine
      </button>
      <button 
        :class="{ ativo: paginaAtual === 'cadastro' }" 
        @click="emit('mudar-pagina', 'cadastro')"
      >
        Novo Item +
      </button>
    </nav>
  </header>
</template>

<style scoped>
.cabecalho {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  border-bottom: 1px solid var(--borda-suave);
  margin-bottom: 30px;
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
  color: var(--cor-texto);
}

.menu-navegacao {
  display: flex;
  gap: 15px;
}

button {
  padding: 10px 20px;
  border-radius: var(--raio-borda);
  background-color: transparent;
  border: 1px solid var(--borda-suave);
  color: var(--cor-texto);
  font-weight: 600;
  transition: all 0.3s;
}

button:hover {
  background-color: var(--borda-suave);
}

/* A classe que deixa o botão laranja quando está na página certa */
button.ativo {
  background-color: var(--cor-primaria);
  border-color: var(--cor-primaria);
  color: white;
}
</style>