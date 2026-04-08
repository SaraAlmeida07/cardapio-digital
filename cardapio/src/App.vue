<script setup>
import { ref, computed } from 'vue'

import CabecalhoApp from './components/CabecalhoApp.vue'
import FormularioItem from './components/FormularioItem.vue'  


// Exemplo de produto para a vitrine
const produtos = ref ([
  { id: 1, nome: 'Hambúrguer Clássico', preco: 25.00, categoria: 'Lanche', disponivel: true, imagem: 'https://images.unsplash.com/photo-1568901346375-23c9450c58cd?q=80&w=500&auto=format&fit=crop' }
])

// Função para adicionar um novo produto à vitrine
function adicionarProduto(produtoRecebido) {
  produtos.value.push(produtoRecebido)
  console.log('Produto adicionado:', produtoRecebido)
}
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
      
      <pre style="color: var(--cor-texto-mutado);">{{ produtos }}</pre>
    </main>

    </div>

  <div class="container">
    <main v-if="paginaAtual === 'cadastro'">
      <h2>Cadastrar Novo Produto</h2>

      <FormularioItem @adicionar="adicionarProduto" />

    </main>

    <main v-else-if="paginaAtual === 'vitrine'">
      <h2>Vitrine de Produtos</h2>
      <div v-if="produtos.length === 0">
        <p>Nenhum produto cadastrado. Volte para a página de cadastro para adicionar produtos.</p>
      </div>

      <pre style="color: white;">{{ produtos }}</pre>

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
</style>