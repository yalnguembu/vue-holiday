<template>
  <template v-if="isUserConnected()">
    <TheMainNavbar />
    <RouterView />
  </template>
</template>

<script setup>
import { onMounted } from "@vue/runtime-core";
import TheMainNavbar from "./TheMainNavbar.vue";
import { useRouter } from "vue-router";

const router = useRouter();
const filteredUser = (userSession) => {
  const users = JSON.parse(localStorage.getItem("users"));
  return users.filter((user) => user.email === userSession.email);
};
const isUserConnected = () => {
  const userSession =
    localStorage.getItem("user") || sessionStorage.getItem("user");
  return userSession && filteredUser(JSON.parse(userSession)).length > 0;
};

onMounted(() => {
  !isUserConnected() && router.push("/login");
});
</script>
