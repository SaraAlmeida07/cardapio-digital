<script setup>
    import { reactive } from 'vue'
    // 1. Avisa ao Vue que este componente emite um evento chamado 'adicionar'
    const emit = defineEmits(['adicionar'])

    const form = reactive({
        nome: '',
        preco: '',
        categoria: '',
        disponivel: true,
        imagem:''
    })

  // 2. Função para enviar os dados do formulário para o App.vue
    function salvarProduto() {
        const novoProduto = {
            id: crypto.randomUUID(),
            nome: form.nome,
            preco: parseFloat(form.preco),
            categoria: form.categoria,
            disponivel: form.disponivel,
            imagem: form.imagem
        }
        emit('adicionar', novoProduto)

        // Limpar o formulário após salvar
        form.nome = ''
        form.preco = ''
        form.categoria = ''
        form.disponivel = true
        form.imagem = ''
    }

</script>

<template>
    <div class="card-formulario">
        <form>
        
        <div class="grupo-input">
            <label>URL da imagem:</label>
            <input type="text" v-model="form.imagem" />
        </div>
        <div class="grupo-input">
            <label>Nome do Item:</label>
            <input type="text" v-model="form.nome" />
        </div>

        <div class="grupo-input">
            <label>Preço (R$):</label>
            <input type="number" step="0.01" v-model="form.preco" />
        </div>

        <div class="grupo-input">
            <label>Categoria:</label>
            <select v-model="form.categoria">
                <option value="Lanche">🍔 Lanche</option>
                <option value="Bebida">🥤 Bebida</option>
                <option value="Sobremesa">🍰 Sobremesa</option>
            </select>
        </div>

        <div class="grupo-input">
            <label>Disponível para venda?</label>
            <input type="checkbox" v-model="form.disponivel" />
        </div>

        <button type="submit" class="btn-salvar">Adicionar ao Cardápio</button>

        </form>
  </div>
  </template>

  <style scoped>
/* Estilos básicos para o formulário de cadastro */
.card-formulario {
  background-color: var(--cor-fundo-card);
  padding: 20px;
  border-radius: 8px;
  max-width: 400px;
}
.grupo-input {
  margin-bottom: 15px;
}
.btn-salvar {
  width: 100%;
  padding: 10px;
  background-color: var(--cor-primaria);
  color: white;
  border-radius: 4px;
}
</style>