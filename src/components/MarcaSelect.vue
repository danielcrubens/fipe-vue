<template>
  <div>
    <label for="Marca" class="block text-xl font-medium text-[#00dc82]"> Marca </label>
    <select name="marca" id="marca" :value="props.modelValue" @input="$emit('update:modelValue', $event.target.value)"
      class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
      <option disabled value="">Selecione uma marca</option>
      <option v-for="marca in props.marcas" :value="marca.codigo" :key="marca.codigo">
        {{ marca.nome }}
      </option>
    </select>
  </div>
</template>

<script setup lang="ts">
interface Marca {
  codigo: string;
  nome: string;
}
interface MarcaSelectProps {
  marcas: Array<Marca>;
  modelValue: string;
}

const props = defineProps<MarcaSelectProps>();
</script>
