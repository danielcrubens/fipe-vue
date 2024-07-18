<template>
  <div class="w-11/12 md:w-8/12 2-xl:w-11/12 mx-auto">
    <div class="py-12">
      <h1 class="text-white font-bold py-4 text-xl md:text-3xl">Tabela Fipe</h1>
      <TipoDeVeiculoSelect :modelValue="tipoDeVeiculo" @update:modelValue="tipoDeVeiculo = $event" />
    </div>

    <div class="grid grid-cols-1 gap-4 lg:grid-cols-3 lg:gap-8">
      <MarcaSelect :marcas="marcas" :modelValue="marca" @update:modelValue="marca = $event" />
      <ModeloSelect :modelos="modelos" :modelValue="modelo" @update:modelValue="modelo = $event" />
      <AnoSelect :anos="anos" :modelValue="ano" @update:modelValue="ano = $event" />
    </div>

    <DadosSelecionados v-if="dadosSelecionados" :dados="{
      modeloSelecionado,
      nomeVeiculo,
      codigoFipe,
      valor,
      mesReferencia}" 
      />
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from 'vue';
import TipoDeVeiculoSelect from '@/components/TipoDeVeiculoSelect.vue';
import MarcaSelect from '@/components/MarcaSelect.vue';
import ModeloSelect from '@/components/ModeloSelect.vue';
import AnoSelect from '@/components/AnoSelect.vue';
import DadosSelecionados from '@/components/DadosSelecionados.vue';


const marcas = ref<Marcas[]>([]);
const modelos = ref<Modelos[]>([]);
const anos = ref<Anos[]>([]);
const tipoDeVeiculo = ref<string>('');
const marca = ref<string>('');
const modelo = ref<string>('');
const ano = ref<string>('');
const valor = ref<string>('');
const codigoFipe = ref<string>('');
const nomeVeiculo = ref<string>('');
const mesReferencia = ref<string>('');
const modeloSelecionado = ref<string>('');
const dadosSelecionados = ref<boolean>(false);

const fetchMarcas = async () => {
  try {
    if (tipoDeVeiculo.value) {
      const response = await fetch(`https://parallelum.com.br/fipe/api/v1/${tipoDeVeiculo.value}/marcas`);
      if (!response.ok) throw new Error('Erro ao buscar marcas');
      marcas.value = await response.json();
    }
  } catch (error) {
    console.error(error);
  }
};

const fetchModelos = async () => {
  try {
    if (marca.value) {
      const response = await fetch(`https://parallelum.com.br/fipe/api/v1/${tipoDeVeiculo.value}/marcas/${marca.value}/modelos`);
      if (!response.ok) throw new Error('Erro ao buscar modelos');
      const json = await response.json();
      modelos.value = json.modelos;
    }
  } catch (error) {
    console.error(error);
  }
};

const fetchAnos = async () => {
  try {
    if (modelo.value) {
      const response = await fetch(`https://parallelum.com.br/fipe/api/v1/${tipoDeVeiculo.value}/marcas/${marca.value}/modelos/${modelo.value}/anos`);
      if (!response.ok) throw new Error('Erro ao buscar anos');
      anos.value = await response.json();
    }
  } catch (error) {
    console.error(error);
  }
};

const fetchValor = async () => {
  try {
    dadosSelecionados.value = false;
    if (ano.value) {
      valor.value = '';
      const response = await fetch(`https://parallelum.com.br/fipe/api/v1/${tipoDeVeiculo.value}/marcas/${marca.value}/modelos/${modelo.value}/anos/${ano.value}`);
      if (!response.ok) throw new Error('Erro ao buscar valor');
      const json = await response.json();
      valor.value = json.Valor;
      codigoFipe.value = json.CodigoFipe;
      nomeVeiculo.value = json.Modelo;
      modeloSelecionado.value = json.Marca;
      mesReferencia.value = json.MesReferencia;
      dadosSelecionados.value = true;
    }
  } catch (error) {
    console.error(error);
  }
};

watch(tipoDeVeiculo, () => {
  marca.value = modelo.value = ano.value = valor.value = '';
  fetchMarcas();
});

watch(marca, () => {
  modelo.value = ano.value = valor.value = '';
  fetchModelos();
});

watch(modelo, () => {
  ano.value = valor.value = '';
  fetchAnos();
});

watch(ano, fetchValor);
onMounted(fetchMarcas);
</script>