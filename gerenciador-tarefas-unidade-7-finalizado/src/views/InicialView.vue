<script setup>
import { ref, computed, onMounted } from 'vue';

// Estados reativos da aplicação
const tarefas = ref([]);
const filtroAtual = ref('todas'); // Pode ser: 'todas', 'pendentes' ou 'concluidas'

// Simulação de busca inicial de dados (Consumo de API de referência)
onMounted(async () => {
  
  tarefas.value = [
    { id: 1, titulo: 'Estudar Vue.js e SPA', concluida: false },
    { id: 2, titulo: 'Configurar rotas do Express', concluida: true },
    { id: 3, titulo: 'Ajustar persistência no MariaDB', concluida: false }
  ];
});

// Lógica de Filtro usando Propriedade Computada (Simples e Eficiente)
const tarefasFiltradas = computed(() => {
  if (filtroAtual.value === 'pendentes') {
    return tarefas.value.filter(t => !t.concluida);
  }
  if (filtroAtual.value === 'concluidas') {
    return tarefas.value.filter(t => t.concluida);
  }
  return tarefas.value; // Retorna 'todas'
});

// Alternar status de forma reativa
function alternarStatus(tarefa) {
  tarefa.concluida = !tarefa.concluida;
}
</script>

<template>
  <div class="gerenciador-container">
    <h2>Meu Gerenciador de Tarefas</h2>

    <!-- Controles do Filtro Obrigatório -->
    <div class="filtros-abas">
      <button 
        :class="{ ativo: filtroAtual === 'todas' }" 
        @click="filtroAtual = 'todas'">Todas</button>
      <button 
        :class="{ ativo: filtroAtual === 'pendentes' }" 
        @click="filtroAtual = 'pendentes'">Pendentes</button>
      <button 
        :class="{ ativo: filtroAtual === 'concluidas' }" 
        @click="filtroAtual = 'concluidas'">Concluídas</button>
    </div>

    <!-- Lista de Tarefas com Animação Visual Nativa (TransitionGroup) -->
    <ul class="lista-tarefas">
      <TransitionGroup name="fade">
        <li 
          v-for="tarefa in tarefasFiltradas" 
          :key="tarefa.id" 
          class="item-tarefa"
          :class="{ concluida: tarefa.concluida }"
          @click="alternarStatus(tarefa)">
          <span>{{ tarefa.titulo }}</span>
          <small>({{ tarefa.concluida ? 'Concluída' : 'Pendente' }})</small>
        </li>
      </TransitionGroup>
    </ul>
  </div>
</template>

<style scoped>
.gerenciador-container {
  max-width: 500px;
  margin: 20px auto;
  font-family: sans-serif;
}
.filtros-abas {
  margin-bottom: 15px;
}
.filtros-abas button {
  margin-right: 5px;
  padding: 8px 12px;
  cursor: pointer;
  border: 1px solid #ccc;
  background-color: #fff;
}
.filtros-abas button.ativo {
  background-color: #42b983;
  color: white;
  border-color: #42b983;
}
.lista-tarefas {
  list-style: none;
  padding: 0;
}
.item-tarefa {
  padding: 10px;
  border-bottom: 1px solid #eee;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
}
.item-tarefa.concluida span {
  text-decoration: line-through;
  color: #888;
}

/* Transições Visuais Obrigatórias para a Mudança de Lista (Fade Animation) */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateX(10px);
}
</style>
