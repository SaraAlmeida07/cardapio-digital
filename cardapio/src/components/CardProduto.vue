<script setup>
// O cartão recebe um ÚNICO produto por vez através das Props
defineProps({
  produto: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['remover'])
</script>

<template>
  <div class="card" :class="{ 'esgotado': !produto.disponivel }">
    
    <div class="imagem-container">
      <img :src="produto.imagem" :alt="produto.nome" />
    </div>

    <div class="info">
      <h4 class="nome">{{ produto.nome }}</h4>
      <p class="categoria">{{ produto.categoria }}</p>
      
      <div class="rodape-card">
        <span class="preco">R$ {{ produto.preco.toFixed(2) }}</span>
        
        <span v-if="!produto.disponivel" class="selo-esgotado">Esgotado</span>
        <button class="btn-modelo-remover" @click="emit('remover', produto.id)">
          Remover
        </button>
      </div>
    </div>

  </div>
</template>

<style scoped>
.card {
  background-color: var(--cor-fundo-card);
  border-radius: var(--raio-borda);
  border: 1px solid var(--borda-suave);
  overflow: hidden; /* Corta as bordas da imagem para não vazar do arredondamento */
  transition: transform 0.2s, box-shadow 0.2s;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: var(--sombra-card);
}

.card.esgotado {
  opacity: 0.6; /* Deixa o cartão meio transparente */
  filter: grayscale(80%); /* Deixa a foto quase em preto e branco */
}

.imagem-container {
  width: 100%;
  height: 160px; /* Altura fixa para todas as fotos ficarem iguais */
}

.imagem-container img {
  width: 100%;
  height: 100%;
  object-fit: cover; /* Garante que a foto preencha o espaço sem amassar */
}

.info {
  padding: 15px;
}

.nome {
  font-size: 1.1rem;
  color: var(--cor-texto-principal);
  margin-bottom: 5px;
}

.categoria {
  font-size: 0.85rem;
  color: var(--cor-texto-mutado);
  margin-bottom: 15px;
}

.rodape-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.preco {
  font-weight: bold;
  font-size: 1.2rem;
  color: var(--cor-texto-principal);
}

.selo-esgotado {
  background-color: var(--cor-perigo-fundo);
  color: var(--cor-perigo);
  font-size: 0.75rem;
  padding: 4px 8px;
  border-radius: 4px;
  font-weight: bold;
}

/* --- NOVO ESTILO DO BOTÃO DE REMOVER (BASEADO NO MODELO) --- */
.btn-modelo-remover {
  background-color: rgb(160, 40, 40); /* Tom de vermelho escuro/marrom escuro do modelo */
  color: white; /* Texto branco */
  border: none; /* Sem borda */
  cursor: pointer; /* Cursor de clique */
  font-size: 0.8rem; /* Fonte pequena */
  padding: 4px 10px; /* Pequeno padding retangular */
  border-radius: 4px; /* Cantos arredondados */
  font-weight: bold; /* Texto em negrito (opcional, mas ajuda na leitura) */
  transition: background-color 0.2s; /* Transição suave de cor no hover */
}

.btn-modelo-remover:hover {
  background-color: rgb(180, 50, 50); /* Clareia sutilmente no hover */
}
</style>