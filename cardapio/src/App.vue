<script setup>
import { ref, computed } from 'vue'

import CabecalhoApp from './components/CabecalhoApp.vue'
import FormularioItem from './components/FormularioItem.vue'
import GradeProdutos from './components/GradeProdutos.vue'  

// Define o array padrão de produtos
const produtoPadrao = []

const dadosSalvos = localStorage.getItem('meu_cardapio')
const produtos = ref(dadosSalvos ? JSON.parse(dadosSalvos) : produtoPadrao)


// Função para adicionar um novo produto à vitrine
function adicionarProduto(produtoRecebido) {
  produtos.value.push(produtoRecebido)
  localStorage.setItem('meu_cardapio', JSON.stringify(produtos.value))
  console.log('Produto adicionado:', produtoRecebido)
}

// Função para remover um produto da vitrine com pop-up de confirmação

function removerProduto(id) {
  if (confirm('Tem certeza que deseja remover este produto?')) {
    produtos.value = produtos.value.filter(produto => produto.id !== id)
    localStorage.setItem('meu_cardapio', JSON.stringify(produtos.value))
    console.log('Produto removido:', id)
  } else {
    console.log('Remoção cancelada para o produto:', id)
  } 
}

//filtro por categoria



</script>


<template>

  <CabecalhoApp />
    <div class="container-dashboard">
    
    <aside class="coluna-esq">
      <h3 class="titulo-coluna">Adicionar Novo Item</h3>
      
      <FormularioItem @adicionar="adicionarProduto" />
    </aside>

    <main class="coluna-dir">
      <h3 class="titulo-coluna">Vitrine do Cardápio</h3>

      <div v-if="produtos.length === 0" class="estado-vazio">
        <span class="icone-vazio">🍽️</span>
        <h4>Nenhum produto cadastrado</h4>
        <p>Use o formulário ao lado para adicionar o primeiro item ao seu cardápio!</p>
      </div>
      
     <GradeProdutos 
        v-else 
        :listaProdutos="produtos" 
        @remover="removerProduto" 
      />
    </main>

    </div>
</template>

<style scoped>/* O container agora usa CSS Grid para criar duas colunas */
.container-dashboard {
  max-width: 1200px;
  margin: 40px auto; /* Afasta um pouco do cabeçalho */
  padding: 0 20px;
  
  display: grid;
  /* A primeira coluna tem 350px fixos, a segunda ocupa o resto (1fr) */
  grid-template-columns: 350px 1fr; 
  gap: 40px; /* Espaço entre o form e a vitrine */
  align-items: start; /* Impede que o formulário estique para baixo se a vitrine crescer */
}

.titulo-coluna {
  font-size: 1.2rem;
  color: var(--cor-texto-principal);
  margin-bottom: 20px;
}

/* --- ESTILO DO ESTADO VAZIO --- */
.estado-vazio {
  background-color: var(--cor-fundo-card);
  border: 2px dashed var(--borda-suave); /* Borda tracejada dá ideia de espaço reservado */
  border-radius: var(--raio-borda);
  padding: 50px 20px;
  text-align: center;
  color: var(--cor-texto-mutado);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.icone-vazio {
  font-size: 3rem;
  opacity: 0.5;
}

.estado-vazio h4 {
  color: var(--cor-texto-principal);
  font-size: 1.2rem;
}

/* --- RESPONSIVIDADE --- */
/* Quando a tela for menor que 800px (celulares e tablets pequenos) */
@media (max-width: 800px) {
  .container-dashboard {
    /* Muda o grid para ter apenas 1 coluna ocupando 100% do espaço */
    grid-template-columns: 1fr; 
    gap: 30px; /* Reduz um pouco o buraco entre o form e a vitrine */
  }
}
</style>