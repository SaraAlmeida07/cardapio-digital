<script setup>
// Este componente é responsável por exibir uma grade de produtos usando o componente CardProduto
import CardProduto from './CardProduto.vue'
// 2. A Grade recebe a LISTA INTEIRA de produtos do App.vue
defineProps({
  listaProdutos: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['remover'])

</script>

<template>
    <div class="grade-container">
        <!-- 3. Para cada produto da lista, renderiza um CardProduto passando o produto como prop -->
        <CardProduto 
          v-for="produto in listaProdutos" 
          :key="produto.id" 
          :produto="produto"
          @remover="emit('remover', $event)"
        />
    </div>
</template>

<style scoped>
.grade-container {
  display: grid;
  /* Cria colunas responsivas: no mínimo 250px, se tiver mais espaço, divide igualmente */
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}
</style>