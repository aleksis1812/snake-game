<template>
  <div
    style="display: flex; flex-direction: column; margin: 0; min-height: 98vh"
  >
    <div
      v-for="row in rows"
      style="
        display: table;
        border-collapse: collapse;
        table-layout: fixed;
        width: 100%;
        flex: 1;
      "
    >
      <div
        v-for="col in columns"
        style="display: table-cell; border: 1px solid red"
      >
        <!-- <span style="color: white; display: block">pp</span> -->
      </div>
    </div>
  </div>
</template>

<script setup>
const rows = ref(20);
const columns = ref(50);
</script>
