<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

const estado = reactive({

  filtro: 'Todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estado ES6 ',
      finalizada: false,
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,
    }
  ]
});

const getTarefasPendentes = () => estado.tarefas.filter(tarefa => !tarefa.finalizada);

const getTarefasFinalizadas = () => estado.tarefas.filter(tarefa => tarefa.finalizada);

const getTarefasFiltradas = () => {
  const { filtro } = estado;

  switch (filtro) {
    case 'Pendentes':
      return getTarefasPendentes();
    case 'Finalizadas':
      return getTarefasFinalizadas();
    default:
      return estado.tarefas;
  }
}

const cadastraTarefa = () => {
  const novaTarefa = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefas.push(novaTarefa);
  estado.tarefaTemp = '';
}

</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />

    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp"
      :editaTarefaTemp="evento => estado.tarefaTemp = evento.target.value" :cadastrar-tarefa="cadastraTarefa" />

    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>

