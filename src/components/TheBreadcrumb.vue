<template>
  <h1 class="my-4">
    <RouterLink to="/" data-test="breadcrumb-link">
      <span class="align-middle align-middle mr-2 text-gray-500 text-lg">
        Home
      </span>
    </RouterLink>
    <ArrowRigthIcon class="mr-2 align-middle" data-test="arrow-right" />
    <template v-for="(route, index) in routes" :key="index">
      <RouterLink
        data-test="breadcrumb-link"
        :to="`/${route}`"
        v-if="isNotTheLast(index)"
      >
        <span class="text-gray-500 text-lg mr-2 align-middle">
          {{ route }}</span
        >
      </RouterLink>
      <span
        v-else
        class="align-middle mr-2 text-black text-2xl font-bold"
        data-test="last-item"
      >
        {{ isHolidayDetailsPage ? `holiday details` : route }}
      </span>
      <ArrowRigthIcon
        v-if="isNotTheLast(index)"
        class="mr-2 align-middle"
        data-test="arrow-right"
      />
    </template>
  </h1>
</template>

<script setup>
import ArrowRigthIcon from "./icons/ArrowRigthIcon.vue";
import { useRoute } from "vue-router";
import { computed } from "@vue/runtime-core";

const route = useRoute();

const isNotTheLast = (index) => {
  return index < routes.value.length - 1;

};

const routes = computed(() => {
  return route.path.split("/").splice(1);
});
const isHolidayDetailsPage = computed(() => {
  return route.params.id ? true : false;
});
</script>
