<script setup lang="ts">
import { useNav } from "@slidev/client";
import type { SlideRoute } from "@slidev/types";
import { computed } from "vue";

const props = defineProps<{
  page: number;
}>();

const nav = useNav();

const parentRoutes = computed(() => {
  const routes = nav.slides.value;
  const currentRoute = routes[props.page - 1];
  const previousRoutes = routes.slice(0, props.page - 1);

  let currentLevel = currentRoute.meta.slide.level;
  return previousRoutes.reverse().reduce((acc, route) => {
    if (currentLevel === 1) return acc;

    if (route.meta.slide.level === currentLevel - 1) {
      currentLevel = route.meta.slide.level;
      return [route, ...acc];
    }
    return acc;
  }, [] as SlideRoute[]);
});
</script>

<template>
  <div
    class="fixed top-2 left-6 text-base flex gap-1 items-center text-gray-500 dark:text-gray-300"
  >
    <template v-for="(route, index) in parentRoutes" :key="route.no">
      <span>{{ route.meta.slide.title }}</span>
      <span v-if="index !== parentRoutes.length - 1">/</span>
    </template>
  </div>
</template>
