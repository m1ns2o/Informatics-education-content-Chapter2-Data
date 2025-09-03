<template>
  <div class="flex justify-between items-center mt-8 pt-6 border-t border-gray-200">
    <UButton
      v-if="previousPage"
      :to="previousPage.path"
      variant="ghost"
      icon="i-lucide-arrow-left"
      size="lg"
    >
      {{ previousPage.label }}
    </UButton>
    <div v-else></div>

    <UButton
      to="/"
      variant="outline"
      icon="i-lucide-home"
      size="sm"
    >
      홈으로
    </UButton>

    <UButton
      v-if="nextPage"
      :to="nextPage.path"
      variant="ghost"
      icon="i-lucide-arrow-right"
      trailing
      size="lg"
    >
      {{ nextPage.label }}
    </UButton>
    <div v-else></div>
  </div>
</template>

<script setup lang="ts">
interface PageInfo {
  path: string
  label: string
}

interface Props {
  previousPage?: PageInfo
  nextPage?: PageInfo
}

defineProps<Props>()
</script>