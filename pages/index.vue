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
        style="display: table-cell; border: 1px solid green"
        :style="{
          background:
            snake.filter((part) => part.x == row && part.y == col).length ||
            (food.x == row && food.y == col)
              ? 'green'
              : 'black',
        }"
      ></div>
    </div>
  </div>
</template>

<script setup>
const rows = ref(20);
const columns = ref(45);
const snake = ref([{ x: 10, y: 20 }]);
const food = ref({ x: 5, y: 6 });
const direction = ref({ x: 0, y: 0 });

onMounted(() => {
  window.addEventListener("keyup", function (ev) {
    console.log(ev.key);
    switch (ev.key) {
      case "ArrowUp": {
        direction.value = { x: -1, y: 0 };
        break;
      }
      case "ArrowDown": {
        direction.value = { x: 1, y: 0 };
        break;
      }
      case "ArrowRight": {
        direction.value = { x: 0, y: 1 };
        break;
      }
      case "ArrowLeft": {
        direction.value = { x: 0, y: -1 };
        break;
      }
    }
    move();
  });

  setInterval(move, 500);
});

function move() {
  let newHead = {
    x: snake.value[0].x + direction.value.x,
    y: snake.value[0].y + direction.value.y,
  };

  snake.value = snake.value.map((part, index) => {
    if (index == 0) return newHead;

    return snake.value[index - 1];
  });
}
</script>
