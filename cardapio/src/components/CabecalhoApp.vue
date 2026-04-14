<script setup>

</script>

<template>
  <header class="cabecalho-fundo">
    <div class="cabecalho-conteudo">
      <div class="logo-titulo">
        <img src="/src/assets/logoCd.png" alt="Logo CD Food" class="icone">
        <h1>Cardápio Digital</h1>
      </div>
    </div>
  </header>
</template>

<style scoped>
.cabecalho-fundo {
  background-color: transparent;
  border-bottom: 1px solid var(--borda-suave);
  border-radius: 10px;
  width: 100%;
}

.cabecalho-conteudo {
  max-width: 100%; 
  margin: 0 auto;
  padding: 10px 20px;
  display: flex;
  align-items: center; 
}

.logo-titulo {
  display: flex;
  align-items: center;
  gap: 15px;
}

.icone {
  height: 65px; /* Trava a altura da logo */
  width: auto; /* Garante que ela não fique esmagada ou esticada */
  object-fit: contain;
}

h1 {
  font-size: 1.5rem;
  color: var(--cor-texto-principal);
}
</style>