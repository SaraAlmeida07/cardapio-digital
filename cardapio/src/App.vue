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
const filtroAtivo = ref('Todas')

const produtosFiltrados = computed(() => {
  if (filtroAtivo.value === 'Todas') {
    return produtos.value
  } else {
    return produtos.value.filter(produto => produto.categoria === filtroAtivo.value)
  }
})

//calculadoras do dashboard
const totalProdutos = computed(() => produtos.value.length)
const produtosDisponiveis = computed(() => produtos.value.filter(p => p.disponivel).length)

//preço médio dos produtos filtrados 
const precoMedio = computed(() => {
  if (produtosFiltrados.value.length === 0) return 0
  const somaPrecos = produtosFiltrados.value.reduce((soma, produto) => soma + produto.preco, 0)
  return somaPrecos / produtosFiltrados.value.length
})

</script>


<template>

  <CabecalhoApp />
    
  <section class="dashboard-resumo">
    <h3 class="titulo-coluna">Dashboard Resumo</h3>
    
    <div class="cards-resumo">
      <div class="card-dado">
        <span class="icone-dado">📄</span>
        <div class="info-dado">
          <p>Total de Produtos :</p>
          <strong>{{ totalProdutos }}</strong>
        </div>
      </div>

      <div class="card-dado">
        <span class="icone-dado">✅</span>
        <div class="info-dado">
          <p>Itens Disponíveis:</p>
          <strong>{{ produtosDisponiveis }}</strong>
        </div>
      </div>

      <div class="card-dado">
        <span class="icone-dado">💰</span>
        <div class="info-dado">
          <p>Preço Médio ({{ filtroAtivo }}):</p>
          <strong>R$ {{ precoMedio.toFixed(2) }}</strong>
        </div>
      </div>
    </div>
  </section>
    
  <div class="container-dashboard">
    
    <aside class="coluna-esq">
      <h3 class="titulo-coluna">Adicionar Novo Item</h3>
      
      <FormularioItem @adicionar="adicionarProduto" />
    </aside>

    <main class="coluna-dir">

      <div class="filtros-container">
        <h3 class="titulo-coluna">Filtros de Categoria</h3>
        <div class="botoes-filtro">
          <button 
            class="btn-filtro" 
            :class="{ ativo: filtroAtivo === 'Todas' }" 
            @click="filtroAtivo = 'Todas'"
          >
            Todas
          </button>
          <button 
            class="btn-filtro" 
            :class="{ ativo: filtroAtivo === 'Lanche' }" 
            @click="filtroAtivo = 'Lanche'"
          >
            🍔 Lanches
          </button>
          <button 
            class="btn-filtro" 
            :class="{ ativo: filtroAtivo === 'Bebida' }" 
            @click="filtroAtivo = 'Bebida'"
          >
            🥤 Bebidas
          </button>
          <button 
            class="btn-filtro" 
            :class="{ ativo: filtroAtivo === 'Sobremesa' }" 
            @click="filtroAtivo = 'Sobremesa'"
          >
            🍰 Sobremesas
          </button>

        </div>
      </div>
      <h3 class="titulo-coluna">Vitrine do Cardápio</h3>

      <div v-if="produtos.length === 0" class="estado-vazio">
        <span class="icone-vazio">🍽️</span>
        <h4>Nenhum produto cadastrado</h4>
        <p>Use o formulário ao lado para adicionar o primeiro item ao seu cardápio!</p>
      </div>
      
     <GradeProdutos 
        v-else 
        :listaProdutos="produtosFiltrados" 
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

/* --- ESTILOS DOS FILTROS --- */
.filtros-container {
  margin-bottom: 30px;
}

.botoes-filtro {
  display: flex;
  gap: 15px;
  flex-wrap: wrap; /* Garante que os botões caiam para a linha de baixo se a tela for pequena */
}

.btn-filtro {
  background-color: transparent; /* Fundo transparente por padrão */
  color: var(--cor-texto-principal);
  border: 1px solid var(--borda-suave);
  padding: 8px 18px;
  border-radius: 20px; /* Isso que cria o formato de 'pílula' */
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.2s;
}

.btn-filtro:hover {
  border-color: var(--cor-primaria); /* Acende a borda no hover */
}

/* A CLASSE MÁGICA: Só é ativada no botão clicado */
.btn-filtro.ativo {
  background-color: var(--cor-primaria);
  color: white;
  border-color: var(--cor-primaria);
  font-weight: bold;
}

/* --- ESTILOS DO DASHBOARD RESUMO --- */

.dashboard-resumo {
  grid-column: 1 / -1; 
  margin: 20px; /* Dei um espacinho a mais aqui para separar dos botões */
}

/* 1. MUDAMOS DE FLEX PARA GRID AQUI */
.cards-resumo {
  display: grid;
  /* Se tiver espaço, coloca lado a lado. Se a tela for menor que 250px, empilha! */
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.card-dado {
  /* Apagamos o flex: 1 e o min-width que estavam aqui */
  background-color: var(--cor-fundo-card);
  padding: 15px 20px;
  border-radius: var(--raio-borda);
  border: 1px solid var(--borda-suave);
  display: flex;
  align-items: center;
  gap: 15px;
}

.icone-dado {
  display: flex;
  flex-direction: column;
  justify-content: center;
  font-size: 1.8rem;
  background-color: var(--borda-suave);
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 12px;
  opacity: 0.8;
}

.info-dado p {
  color: var(--cor-texto-mutado);
  font-size: 0.85rem;
  margin-bottom: 2px;
}

.info-dado strong {
  color: var(--cor-texto-principal);
  font-size: 1.4rem;
}

/* --- RESPONSIVIDADE --- */
/* Quando a tela for menor que 700px (celulares e tablets pequenos) */
@media (max-width: 700px) {
  .container-dashboard {
    /* Muda o grid para ter apenas 1 coluna ocupando 100% do espaço */
    grid-template-columns: 1fr; 
    gap: 30px; /* Reduz um pouco o buraco entre o form e a vitrine */
  }
  .cards-resumo {
    grid-template-columns: 1fr;
  }
}


</style>