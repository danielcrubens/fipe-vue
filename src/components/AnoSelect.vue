<template>
  <div>
    <label for="ano" class="block text-xl font-medium text-[#00dc82]"> Ano </label>
    <select name="ano" id="ano" :value="props.modelValue" @input="$emit('update:modelValue', $event.target.value)"
      class="mt-1.5 w-full rounded-lg border-gray-300 text-gray-700 sm:text-sm py-3">
      <option disabled value="">Selecione um ano</option>
      <option v-for="ano in props.anos" :value="ano.codigo" :key="ano.codigo">
        {{ ano.nome }}
      </option>
    </select>
  </div>
</template>

<script setup lang="ts">
interface Ano {
  codigo: string;
  nome: string;
}
interface AnoSelectProps {
  anos: Array<Ano>;
  modelValue: string;
}

const props = defineProps<AnoSelectProps>();
</script>

