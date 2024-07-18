<template>
  <div>
    <label for="modelo" class="block text-xl font-medium text-[#00dc82]"> Modelo </label>
    <select name="modelo" id="modelo" :value="props.modelValue" @input="$emit('update:modelValue', $event.target.value)"
      class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
      <option disabled value="">Selecione um modelo</option>
      <option v-for="modelo in props.modelos" :value="modelo.codigo" :key="modelo.codigo">
        {{ modelo.nome }}
      </option>
    </select>
  </div>
</template>

<script setup lang="ts">

interface Modelos {
  codigo: string;
  nome: string;
}

interface ModelosSelectProps {
  modelos: Array<Modelos>;
  modelValue: string;
}

const props = defineProps<ModelosSelectProps>();
</script>

