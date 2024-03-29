<template>
  <main class="p-4 md:px-12 py-8">
    <template v-if="state.holiday">
      <TheBreadcrumb />
      <div class="mt-8">
        <h2 class="font-bold text-2xl text-[#222]" data-test="date-range">
          {{ dateRange }}
        </h2>
        <p class="text-gray-500 hidden md:block text-center md:text-left" data-test="number-of-days">
          Il vous reste 21 jours
        </p>
      </div>
      <div class="flex flex-row flex-wrap gap-y-4 mt-4">
        <div class="mt-4 basis-full md:basis-1/2 px-2">
          <h6 class="text-gray-500">Type</h6>
          <p class="font-bold" data-test="holiday-type">{{ state.holiday?.holidayType }}</p>
        </div>
        <div class="mt-4 basis-full md:basis-1/2 px-2">
          <h6 class="text-gray-500">Starts</h6>
          <p class="flex flex-row justify-between items-center pr-8">
            <span class="font-bold" data-test="starting-date">{{
              startingDate
            }}</span>
            <AgendaIcon />
          </p>
        </div>
        <div class="mt-4 hidden basis-full md:basis-1/2 px-2 md:block">
          <h6 class="text-gray-500">Return</h6>
          <p class="font-bold" data-test="returning-date">
            {{ returningDate }}
          </p>
        </div>
        <div class="mt-4 basis-full md:basis-1/2 px-2">
          <h6 class="text-gray-500">End</h6>
          <p class="flex flex-row justify-between items-center pr-8">
            <span class="font-bold" data-test="ending-date">{{
              endingDate
            }}</span>
            <AgendaIcon />
          </p>
        </div>
        <div class="">
          <h6 class="text-gray-500">Description</h6>
          <p class="font-bold" data-test="holiday-description">
            {{ state.holiday.description }}
          </p>
        </div>
        <div class="mt-4 md:hidden">
          <h6 class="text-gray-500">Return</h6>
          <p class="font-bold" data-test="returning-date">
            {{ returningDate }}
          </p>
        </div>
      </div>
    </template>
    <div class="flex flex-col items-center justify-center py-12" v-else>
      <h1>Oups Holiday not found</h1>
      <router-link
        to="/list"
        class="px-4 py-2 bg-blue-500 text-white font-bold rounded-md mt-6"
      >
        Return to holidays list
      </router-link>
    </div>
  </main>
</template>

<script setup lang="ts">
import TheBreadcrumb from "../../components/TheBreadcrumb.vue";
import AgendaIcon from "../../components/icons/AgendaIcon.vue";
import { computed, onMounted, reactive } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const state = reactive({ holiday: <Holiday | undefined>{} });
type Holiday = {
  holidayType: string;
  id: number;
  startingDate: string;
  returningDate: string;
  endingDate: string;
  description: string;
};

onMounted(() => {
  state.holiday = findHolidayById(
    getHolidays(),
    parseInt(route.params?.id as string)
  );
});

const getHolidays = (): Holiday[] => {
  return JSON.parse(localStorage.getItem("holidays") ?? "") ?? [];
};

const findHolidayById = (holidays: Holiday[], id: number) => {
  return holidays.find((holiday: Holiday) => holiday.id == id);
};
const toDateString = (date: string) => {
  return new Date(date).toDateString();
};

const dateRange = computed(() => {
  return `
      ${toDateString(state.holiday?.startingDate ?? "")}
      - ${toDateString(state.holiday?.returningDate ?? "")}
      `;
});
const returningDate = computed(() => {
  return toDateString(state.holiday?.returningDate ?? "");
});
const startingDate = computed(() => {
  return toDateString(state.holiday?.startingDate ?? "");
});
const endingDate = computed(() => {
  return toDateString(state.holiday?.endingDate ?? "");
});
</script>
