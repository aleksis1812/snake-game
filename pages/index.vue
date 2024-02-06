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
      >
        {{ row }} {{ col }}
      </div>
    </div>
  </div>
</template>

<script setup>
const rows = ref(20);
const columns = ref(45);
const snake = ref([{ x: 10, y: 20 }]);
const food = ref({});
const direction = ref({ x: 0, y: 0 });

onMounted(() => {
  addFood();

  window.addEventListener("keyup", function (ev) {
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
  });

  setInterval(move, 200);
});

function move() {
  let newHead = {
    x: snake.value[0].x + direction.value.x,
    y: snake.value[0].y + direction.value.y,
  };

  if (newHead.x == food.value.x && newHead.y == food.value.y) eat();

  snake.value = snake.value.map((part, index) => {
    if (index == 0) return newHead;
    return snake.value[index - 1];
  });
}

function eat() {
  snake.value.push({});
  addFood();
}

function addFood() {
  let x = 0;
  let y = 0;

  do {
    x = Math.floor(Math.random() * (rows.value - 1)) + 1;
    y = Math.floor(Math.random() * (columns.value - 1)) + 1;
  } while (
    snake.value.filter((part) => part.x == x && part.y == y).length != 0
  );

  console.log(x, y);
  food.value = { x, y };
}
</script>
