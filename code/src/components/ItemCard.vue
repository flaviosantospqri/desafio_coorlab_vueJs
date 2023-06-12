<template>
  <div class="container-card">
    <h4>{{ message }}</h4>
    <div class="container-informations">
      <section class="data-transport">
        <div class="figure">
          <img :src="image" alt="" srcset="" />
        </div>
        <p>
          Transportadora: <span>{{ tranpostName }}</span>
        </p>
        <p>
          Tempo Estimando: <span>{{ leadTime }}</span>
        </p>
      </section>
      <section class="data-value">
        <p>
          Preço: <span>{{ price }}</span>
        </p>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  name: "ItemCardRender",

  props: {
    message: {
      type: String,
    },
    tranpostName: {
      type: String,
    },
    leadTime: {
      type: String,
    },
    price: {
      type: String,
    },
    image: {
      type: String,
    },
  },
};
</script>

<style>
.container-informations {
  width: 100%;
  box-sizing: border-box;
  max-width: 500px;
  max-height: 100px;
  min-width: 350px;
  padding: 5px;
  display: flex;
  justify-content: space-between;
}
.figure {
  padding: 20px;
  background-color: #00aca6;
  border-radius: 4px;
  margin-right: 5px;
}
.data-transport {
  border-radius: 4px;
  display: flex;
  flex-direction: row;
  align-content: center;
  justify-content: center;
  flex: 1;
  background-color: #cccccc;
}
.data-value {
  display: flex;
  max-width: 100px;
  border-radius: 4px;
  align-items: center;
  justify-content: center;
  background-color: #cccccc;
  padding: 10px;
  margin-left: 10px;
}
.top-message {
  display: flex;
  text-align: left;
  width: 80%;
  color: #6c757d;
  font-size: medium;
  font-weight: lighter;
}
.container-card {
  display: flex;
  justify-content: space-evenly;
  height: 100%;
  flex-direction: column;
  animation: go-back 1s ease alternate;
}
@keyframes go-back {
  from {
    transform: translateX(100px);
  }
  to {
    transform: translateX(0);
  }
}
.btn-clear {
  align-self: flex-end;
  background-color: #00aca6;
  width: 100px;
  text-align: center;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
}
p {
  padding: 5px;
}
</style>