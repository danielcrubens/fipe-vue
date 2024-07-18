<template>
  <div class="grid gap-8 grid-cols-2 py-10 mt-12">
    <div class="border-solid border-2 text-white border-white rounded-lg p-4">
      <p class="text-sm mb-2 md:mb-1"> {{ dados.modeloSelecionado }}</p>
      <p class="md:text-lg text-xl font-semibold mb-2 md:mb-1">{{ dados.nomeVeiculo }}</p>
      <p class="text-sm mb-2 md:mb-1"> Código Fipe: <span class="font-bold">{{ dados.codigoFipe }} </span> </p>
    </div>
    <div class="border-solid text-white border-2 border-white rounded-lg p-4">
      <p class="text-sm mb-2 md:mb-1">Preço médio</p>
      <p class="md:text-3xl text-xl font-bold mb-2 md:mb-1">{{ dados.valor }}</p>
      <p class="text-sm mb-2 md:mb-1">Mês de referência: {{ dados.mesReferencia }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
interface Dados {
  modeloSelecionado: string;
  nomeVeiculo: string;
  codigoFipe: string;
  valor: string;
  mesReferencia: string;
}
interface DadosSelecionadosProps {
  dados: Dados;
}
const props = defineProps<DadosSelecionadosProps>();
</script>
