<script setup lang="ts">
import { ref } from "vue";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";

const stats = ref([
  {
    title: "Total Studios",
    value: "0",
    description: "Active photography studios",
  },
  {
    title: "Total Photographers",
    value: "0",
    description: "Registered photographers",
  },
  {
    title: "Total Affiliates",
    value: "0",
    description: "Active affiliates",
  },
  {
    title: "Total Revenue",
    value: "$0",
    description: "Platform revenue",
  },
]);
</script>

<template>
  <div class="flex-1 space-y-4 p-8 pt-6">
    <div class="flex items-center justify-between space-y-2">
      <h2 class="text-3xl font-bold tracking-tight">Dashboard</h2>
    </div>
    <div class="grid gap-4 md:grid-cols-2 lg:grid-cols-4">
      <Card v-for="stat in stats" :key="stat.title">
        <CardHeader class="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle class="text-sm font-medium">
            {{ stat.title }}
          </CardTitle>
        </CardHeader>
        <CardContent>
          <div class="text-2xl font-bold">{{ stat.value }}</div>
          <p class="text-xs text-muted-foreground">
            {{ stat.description }}
          </p>
        </CardContent>
      </Card>
    </div>
  </div>
</template> 