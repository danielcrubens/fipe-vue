<template>
  <div class="w-11/12 md:w-8/12 2-xl:w-11/12 mx-auto">
    <div class=" py-12">
      <h1 class="text-white font-bold py-4text-xl md:text-3xl">Tabela Fipe</h1>
      <select name="tipoDeVeiculo" id="tipoDeVeiculo" v-model="tipoDeVeiculo"
        class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
        <option disabled value="">Selecione um tipo de veículo</option>
        <option value="carros">🚗 Carro</option>
        <option value="motos">🏍️ Moto</option>
        <option value="caminhoes">🚚 Caminhão</option>
      </select>
    </div>

    <div class="grid grid-cols-1 gap-4 lg:grid-cols-3 lg:gap-8">
        <div>
          <label for="Marca" class="block text-xl font-medium text-[#00dc82]"> Marca </label>
          <select name="marca" id="marca" v-model="marca"
            class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
            <option disabled value="">Selecione uma marca</option>
            <option v-for="marca in marcas" :value="marca.codigo" :key="marca.codigo">
              {{ marca.nome }}
            </option>
          </select>
        </div>
        <div>
          <label for="Marca" class="block text-xl font-medium text-[#00dc82]"> Modelo </label>
          <select name="marca" id="marca" v-model="marca"
            class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
            <option disabled value="">Selecione um modelo</option>
            <option v-for="marca in marcas" :value="marca.codigo" :key="marca.codigo">
              {{ marca.nome }}
            </option>
          </select>
        </div>
        <div>
          <label for="Marca" class="block text-xl font-medium text-[#00dc82]"> Ano </label>
          <select name="marca" id="marca" v-model="marca"
            class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
            <option disabled value="">Selecione um ano</option>
            <option v-for="marca in marcas" :value="marca.codigo" :key="marca.codigo">
              {{ marca.nome }}
            </option>
          </select>
        </div>
      </div>
  </div>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue';

const marcas = ref({});
const tipoDeVeiculo = ref('');
const marca = ref('');
const clickTipo = ref(false);


const fetchMarcas = async () => {
  try {
    if (tipoDeVeiculo.value) {
      const response = await fetch(`https://brasilapi.com.br/api/fipe/marcas/v1/${tipoDeVeiculo.value}`);
      const data = await response.json();
      marcas.value = data;
    }
  } catch (error) {
    console.error('Erro ao buscar marcas:', error);
  }
};

watch(tipoDeVeiculo, () => {
  marca.value =  '';
  clickTipo.value = true;
  fetchMarcas();
});


onMounted(fetchMarcas);
</script>