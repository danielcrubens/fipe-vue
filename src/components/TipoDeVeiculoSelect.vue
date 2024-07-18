<template>
  <select name="tipoDeVeiculo" id="tipoDeVeiculo" v-model="props.modelValue" @input="$emit('update:modelValue', $event.target.value)" :value="selectedTipoDeVeiculo"
    class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3"
  >
    <option disabled value="">Selecione um tipo de veículo</option>
    <option value="carros">🚗 Carro</option>
    <option value="motos">🏍️ Moto</option>
    <option value="caminhoes">🚚 Caminhão</option>
  </select>
</template>
<script setup lang="ts">
interface Props {
  modelValue: string;
}
const props = defineProps<Props>();
</script>
