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
        <form @submit.prevent="salvarProduto">
        
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

        <div class="grupo-checkbox">
            <span class="label-texto">Disponível para venda:</span>
            
            <label class="switch">
                <input type="checkbox" v-model="form.disponivel" />
                <span class="slider"></span>
            </label>
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
/* --- ESTILOS DO TOGGLE SWITCH --- */
.grupo-checkbox {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
.label-texto {
  color: var(--cor-texto-mutado);
  font-size: 0.9rem;
}

/* Tamanho do botão "pílula" */
.switch {
  position: relative;
  display: inline-block;
  width: 44px;
  height: 24px;
}

/* Esconde o checkbox original (ele continua lá, mas invisível) */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* O fundo do botão (cinza escuro quando desligado) */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: var(--borda-suave); /* Usando a cor da nossa borda */
  transition: 0.4s;
  border-radius: 24px;
}
/* A bolinha branca (feita com o pseudo-elemento ::before) */
.slider::before {
  position: absolute;
  content: "";
  height: 18px;
  width: 18px;
  left: 3px;
  bottom: 3px;
  background-color: white;
  transition: 0.4s;
  border-radius: 50%;
}
/* Quando o checkbox estiver marcado, move a bolinha para a direita e muda a cor do fundo para a cor primária */
.switch input:checked + .slider {
  background-color: var(--cor-primaria);
}
.switch input:checked + .slider::before {
  transform: translateX(20px);
}
</style>