<script setup>
import { ref, computed } from "vue";

const props = defineProps({
  columns: {
    type: [Array, Object],
    required: true,
  },
  rows: {
    type: Array,
    required: true,
  },
  firstLetterUppercase: {
    type: Boolean,
    required: false,
    default: true,
  },
  isFoldable: {
    type: Boolean,
    default: false,
  },
});

const sortColumn = ref("");
const isSortOrderAsc = ref(false);
const search = ref("");

const camelize = (str) => {
  return str
    .replace(/(?:^\w|[A-Z]|\b\w)/g, function (word, index) {
      return index === 0 ? word.toLowerCase() : word.toUpperCase();
    })
    .replace(/\s+/g, "");
};

const sortBy = (type) => {
  if (sortColumn.value === type) {
    isSortOrderAsc.value = !isSortOrderAsc.value;
  }
  sortColumn.value = type;
};

const filteredData = computed(() => {
  return props.rows
    .filter((el) => el.name.toLowerCase().includes(search.value.toLowerCase()))
    .sort((a, b) => {
      let modifier = 1;
      if (!isSortOrderAsc.value) {
        modifier = -1;
      }
      if (a[sortColumn.value] < b[sortColumn.value]) return -1 * modifier;
      if (a[sortColumn.value] > b[sortColumn.value]) return modifier;

      return 0;
    });
});
</script>

<template>
  <table>
    <thead>
      <tr v-if="Array.isArray(columns)">
        <th
          scope="col"
          v-for="column in columns"
          :key="column"
          @click="sortBy(column)"
        >
          <template v-if="firstLetterUppercase">{{
            column.charAt(0).toUpperCase() + column.slice(1)
          }}</template>
          <template v-else>{{ column }}</template>
        </th>
      </tr>

      <tr v-else>
        <th
          scope="col"
          v-for="(column, rawColumnName) in columns"
          :key="rawColumnName"
          @click="sortBy(rawColumnName)"
        >
          {{ column }}
        </th>
      </tr>
    </thead>
    <tbody>
      <template v-for="row in filteredData" :key="row.id">
        <tr v-if="Array.isArray(columns)">
          <td v-for="col in columns" :key="col.id">
            <slot :name="camelize(col)" v-bind="row">{{ row[col] }}</slot>
          </td>
        </tr>

        <tr v-else>
          <td v-for="(col, index) in columns" :key="col">
            <slot :name="camelize(index)" v-bind="row">{{ row[index] }}</slot>
          </td>
        </tr>

        <tr v-if="isFoldable" :id="`tr-panel-${row.id}`">
          <td :colspan="columns.length">Description : {{ row.description }}</td>
        </tr>
      </template>
    </tbody>
  </table>
</template>

<style></style>
