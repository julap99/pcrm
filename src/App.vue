<script setup lang="ts">
import { computed } from "vue";
import { useRoute } from "vue-router";
import { useAuthStore } from "@/stores/auth";
import DashboardLayout from "@/layouts/dashboard.vue";
import BlankLayout from "@/layouts/blank.vue";
import FormsLayout from "@/layouts/forms.vue";
import ClientLayout from "@/layouts/client.vue";
import { Toaster } from "@/components/ui/sonner";

const route = useRoute();
const authStore = useAuthStore();

// Initialize auth store
authStore.init();

const layout = computed<"dashboard" | "blank" | "forms" | "client">(() => {
  return (route.meta.layout as "dashboard" | "blank" | "forms" | "client") || "blank";
});

const layouts = {
  dashboard: DashboardLayout,
  blank: BlankLayout,
  forms: FormsLayout,
  client: ClientLayout,
} as const;
</script>

<template>
  <Toaster position="top-right" />
  <component :is="layouts[layout]">
    <router-view v-slot="{ Component }">
      <transition name="fade-slide" mode="out-in" appear>
        <component :is="Component" :key="route.path" />
      </transition>
    </router-view>
  </component>
</template>

<style scoped>
/* Transition styles only for the current component */
</style>
