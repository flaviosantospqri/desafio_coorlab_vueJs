<template>
  <div class="container">
    <div class="containerForm">
      <h1 class="title">Insira o destino e o peso</h1>
      <form class="form" @submit="getInfomation">
        <div class="input-form-container">
          <label for="selectCity">Destino</label>
          <select name="selectCity" id="city" v-model="city">
            <option selected="selected">Selecione o Destino</option>
            <option v-for="city in citys" :key="city.id" v-bind:value="city">
              {{ city }}
            </option>
          </select>
        </div>

        <div class="input-form-container">
          <label for="weigth">Peso</label>
          <input
            type="text"
            id="weight"
            name="weigth"
            value="weigth"
            v-model="weight"
            placeholder="Peso da Carga em Kg"
          />
        </div>

        <div class="btn-container">
          <input type="submit" class="submit-req-btn" value="Análisar" />
        </div>
      </form>
    </div>
    <div class="container-component">
      <div>
        <ItemCard
          v-if="show"
          :message="msgPrice"
          :leadTime="lowPrice.lead_time"
          :price="valueDelivery"
          :tranpostName="lowPrice.name"
        ></ItemCard>
      </div>
      <div>
        <ItemCard
          v-if="show"
          :message="msgFast"
          :leadTime="fastDelivered.lead_time"
          :price="valueDelivery"
          :tranpostName="fastDelivered.name"
        ></ItemCard>
      </div>
    </div>
    <div class="btn-clear">
        <input
          type="button"
          @click="clear(e)"
          class="submit-req-btn"
          value="Limpar"
        />
      </div>
  </div>
</template>



<script>
import ItemCard from "./ItemCardLowPrice.vue";

export default {
  name: "FormTransportPage",
  components: {
    ItemCard,
  },
  data() {
    return {
      msgPrice: "O mais barato",
      msgFast: "Mais rápido",
      citys: null,
      weight: null,
      datas: null,
      city: "",
      lowPrice: null,
      fastDelivered: null,
      listCities: [],
      show: false,
      valueDelivery: null,
    };
  },

  methods: {
    async getCitys() {
      const req = await fetch("http://localhost:3000/transport");
      const data = await req.json();
      this.datas = data;
      let city = data.map((a) => a.city);
      let diffCity = new Set(city);

      this.citys = Array.from(diffCity).sort();
    },

    async getInfomation(e) {
      e.preventDefault();

      const dataUser = {
        weight: parseInt(this.weight),
        city: this.city,
      };
      this.selectedCity(dataUser);
    },

    selectedCity(dataUser) {
      this.listCities = this.datas.filter((a) => a.city == dataUser.city);
      this.analize();
    },

    calcuteValue(valuePrice) {
      valuePrice = parseFloat(
        valuePrice?.replace(/[^0-9-.]/g, "") * this.weight
      );
      return (this.valueDelivery = new Intl.NumberFormat("pt-BR", {
        style: "currency",
        currency: "BRL",
      }).format(valuePrice));
    },

    findLowPrice() {
      if (this.weight > 100) {
        this.lowPrice = this.listCities.reduce((a, b) => {
          return parseFloat(a.cost_transport_heavy?.replace(/[^0-9-.]/g, "")) <
            parseFloat(b.cost_transport_heavy?.replace(/[^0-9-.]/g, ""))
            ? a
            : b;
        }, 0);
        this.calcuteValue(this.lowPrice.cost_transport_heavy);
      } else {
        this.lowPrice = this.listCities.reduce((a, b) => {
          return parseFloat(a.cost_transport_light) >
            parseFloat(b.cost_transport_light)
            ? a
            : b;
        }, 0);
        this.calcuteValue(this.lowPrice.cost_transport_light);
      }

      return this.lowPrice;
    },

    findFastDelivered() {
      this.fastDelivered = this.listCities.reduce((a, b) => {
        return parseInt(a.lead_time?.replace(/^[0-9]/g, "")) <
          parseInt(b.lead_time?.replace(/^[0-9]/g, ""))
          ? a
          : b;
      }, 0);
      return this.fastDelivered;
    },
    analize() {
      this.show = true;
    },
    clear() {
      this.show = false;
    },
  },

  created() {
    this.getCitys();
  },
  updated() {
    this.findLowPrice();
    this.findFastDelivered();
  },
};
</script>

<style scoped>
.container {
  display: flex;
  width: 100%;
  align-content: center;
  justify-content: space-evenly;
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

.container-component{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  flex: 1;
}
.containerForm {
  display: flex;
  padding: 1.5%;
  margin: 3%;
  flex-direction: column;
  width: 25%;
  justify-content: center;
  align-items: center;
  height: 60vh;
  background-color: #e9ecef;
  border-radius: 8px;
}
.title {
  font-size: 1.6rem;
  margin-bottom: 10%;
}
.form {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  width: 90%;
  height: 50%;
}
.btn-container {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
.submit-req-btn {
  font-weight: bold;
  width: 60%;
  background-color: #00aca6;
}
.input-form-container {
  display: flex;
  flex-direction: column;
}
label {
  font-size: small;
  font-weight: bold;
  color: #222;
}
input,
select {
  padding: 10px;
  font-weight: lighter;
  height: 40px;
  border-radius: 4px;
  border: none;
}
</style>
