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
            snake.filter((part) => part.x == col && part.y == row).length ||
            (food.y == row && food.x == col)
              ? color
              : 'black',
        }"
      ></div>
    </div>
  </div>
</template>

<script setup>
const rows = ref(20);
const columns = ref(45);
const snake = ref([{ x: 20, y: 10 }]);
const food = ref({});
const direction = ref({ x: 0, y: 0 });
const color = ref("green");
const speed = ref(200);
var interval = "";

onMounted(() => {
  addFood();

  window.addEventListener("keyup", function (ev) {
    switch (ev.key) {
      case "ArrowUp": {
        direction.value = { x: 0, y: -1 };
        break;
      }
      case "ArrowDown": {
        direction.value = { x: 0, y: 1 };
        break;
      }
      case "ArrowRight": {
        direction.value = { x: 1, y: 0 };
        break;
      }
      case "ArrowLeft": {
        direction.value = { x: -1, y: 0 };
        break;
      }
    }
  });

  interval = setInterval(move, speed.value);
});

function move() {
  if (direction.value.x == 0 && direction.value.y == 0) return;

  let newX = snake.value[0].x + direction.value.x;
  let newY = snake.value[0].y + direction.value.y;

  if (newX > columns.value) newX = 1;
  if (newX < 1) newX = columns.value;
  if (newY > rows.value) newY = 1;
  if (newY < 1) newY = rows.value;

  let newHead = {
    x: newX,
    y: newY,
  };

  if (newHead.x == food.value.x && newHead.y == food.value.y) eat();

  if (
    snake.value.filter((part) => part.x == newHead.x && part.y == newHead.y)
      .length > 0
  )
    die();

  snake.value = snake.value.map((part, index) => {
    if (index == 0) return newHead;
    return snake.value[index - 1];
  });
}

function eat() {
  snake.value.push({});
  addFood();
  if (speed.value > 20) speed.value -= 10;
  clearInterval(interval);
  interval = setInterval(move, speed.value);
}

function addFood() {
  let x = 0;
  let y = 0;

  do {
    x = Math.floor(Math.random() * (columns.value - 1)) + 1;
    y = Math.floor(Math.random() * (rows.value - 1)) + 1;
  } while (
    snake.value.filter((part) => part.x == x && part.y == y).length != 0
  );

  food.value = { x, y };
}

function die() {
  direction.value = { x: 0, y: 0 };
  color.value = "red";
}
</script>
