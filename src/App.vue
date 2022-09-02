<script setup lang="ts">
import { VueDrawble } from "./components";
import { ref } from "vue";

const rows = ref([
  {
    id: "1",
    name: "My first command",
    description: "I print Hello World on the Terminal",
    "command cli": `echo 'Hello World'`,
  },
  {
    id: "2",
    name: "My second command",
    description: "I print Bonjour on the Terminal",
    "command cli": `echo 'Bonjour'`,
  },
]);
</script>

<template>
  <h1 style="padding-bottom: 3rem">Vue Drawble Example</h1>

  <VueDrawble
    style="padding-bottom: 4rem"
    :rows="rows"
    :columns="['id', 'command cli', 'description']"
  >
    <template #commandCli="{ 'command cli': commandCli }"
      ><span style="color: red">{{ commandCli }}</span></template
    >
  </VueDrawble>

  <VueDrawble
    :rows="rows"
    :columns="{ id: 'ID', name: 'Name', 'command cli': 'Command to execute' }"
  >
    <template #commandCli="{ 'command cli': commandCli }">{{
      commandCli
    }}</template>
    <template #name="{ name }"
      ><a href="#">{{ name }}</a></template
    >
  </VueDrawble>
</template>
