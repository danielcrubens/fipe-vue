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
        <label for="modelo" class="block text-xl font-medium text-[#00dc82]"> Modelo </label>
        <select name="modelo" id="modelo" v-model="modelo"
          class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
          <option disabled value="">Selecione um modelo</option>
          <option v-for="modelo in modelos" :value="modelo.codigo" :key="modelo.codigo">
            {{ modelo.nome }}
          </option>
        </select>
      </div>
      <div>
        <label for="ano" class="block text-xl font-medium text-[#00dc82]"> Ano </label>
        <select name="ano" id="ano" v-model="ano"
          class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
          <option disabled value="">Selecione um ano</option>
          <option v-for="ano in anos" :value="ano.codigo" :key="ano.codigo">
            {{ ano.nome }}
          </option>
        </select>
      </div>
    </div>
    <div v-if="dadosSelecionados" class="grid  gap-8 grid-cols-2 py-10 mt-12">
      <div class="border-solid border-2 text-white border-white rounded-lg p-4">
        <p class="text-sm mb-2 md:mb-1"> {{ modeloSelecionado }}</p>
        <p class="md:text-lg text-xl font-semibold mb-2 md:mb-1">{{ nomeVeiculo }}</p>
        <p class="text-sm mb-2 md:mb-1"> Código Fipe: <span class="font-bold">{{codigoFipe }} </span> </p>


      </div>
      <div class="border-solid text-white border-2 border-white rounded-lg p-4">
  <p class="text-sm mb-2 md:mb-1">Preço médio</p>
  <p class="md:text-3xl text-xl font-bold mb-2 md:mb-1">{{ valor }}</p>
  <p class="text-sm mb-2 md:mb-1">Mês de referência: {{ mesReferencia }}</p>
</div>

    </div>
  </div>
</template>


<script setup>
import { ref, watch, onMounted } from 'vue';

const marcas = ref({});
const modelos = ref({});
const anos = ref({});
const tipoDeVeiculo = ref('');
const marca = ref('');
const modelo = ref('');
const ano = ref('');
const valor = ref('');
const codigoFipe = ref('');
const nomeVeiculo = ref('');
const mesReferencia = ref('');
const modeloSelecionado = ref('');
const clickTipo = ref(false);
const clickMarca = ref(false);
const clickModelo = ref(false);
const dadosSelecionados = ref(false);

const fetchMarcas = async () => {
  try {
    if (tipoDeVeiculo.value) {
      marcas.value = await (
        await fetch(
          `https://parallelum.com.br/fipe/api/v1/${tipoDeVeiculo.value}/marcas`
        )
      ).json();
    }
  } catch (error) {
    console.error('Erro ao buscar marcas:', error);
  }
};

const fetchModelos = async () => {
  try {
    if (marca.value) {
      const json = await (
        await fetch(
          `https://parallelum.com.br/fipe/api/v1/${tipoDeVeiculo.value}/marcas/${marca.value}/modelos`
        )
      ).json();
      modelos.value = json.modelos;
    }
  } catch (error) {
    console.error('Erro ao buscar modelos:', error);
  }
};

const fetchAnos = async () => {
  try {
  if (modelo.value) {
    anos.value = await (
      await fetch(
        `https://parallelum.com.br/fipe/api/v1/${tipoDeVeiculo.value}/marcas/${marca.value}/modelos/${modelo.value}/anos`
      )
    ).json();
  }
  // Tratamento para Veículos Zero KM, pois a API exibe ano "32000" para esses casos
  if (anos.value[0].nome.startsWith('32000'))
    anos.value[0].nome = anos.value[0].nome.replace('32000', 'Zero KM');
  } catch (error) {
    console.error('Erro ao buscar ano:', error);
  }
};

const fetchValor = async () => {
  try {
    dadosSelecionados.value = false;
  if (ano.value) {
    valor.value = '';
    const json = await (
      await fetch(
        `https://parallelum.com.br/fipe/api/v1/${tipoDeVeiculo.value}/marcas/${marca.value}/modelos/${modelo.value}/anos/${ano.value}`
      )
    ).json();
    valor.value = json.Valor;
    codigoFipe.value = json.CodigoFipe;
    nomeVeiculo.value = json.Modelo;
      modeloSelecionado.value = json.Marca;
      mesReferencia.value = json.MesReferencia;
      dadosSelecionados.value = true;
  }
} catch (error) {
    console.error('Erro ao buscar valor:', error);
  }
};

watch(tipoDeVeiculo, () => {
  marca.value = modelo.value = ano.value = valor.value = '';
  clickTipo.value = true;
  fetchMarcas();
});

watch(marca, () => {
  modelo.value = ano.value = valor.value = '';
  clickMarca.value = true;
  fetchModelos();
});

watch(modelo, () => {
  ano.value = valor.value = '';
  clickModelo.value = true;
  fetchAnos();
});

watch(ano, fetchValor);
onMounted(fetchMarcas);
</script>