<template>
  <div class="container">
    <div class="container-form">
      <h1 class="title">
        <span class="imagem-map-tranport"
          ><img
            src="../assets/map-with-clock.svg"
            alt="Imagemm de um mapa com relógio representando transporte" /></span
        >Insira o destino e o peso
      </h1>
      <form class="form" @submit="getInfomation">
        <div class="input-form-container">
          <label for="selectCity">Destino</label>
          <select name="selectCity" id="city" v-model="city">
            <option>Selecione o Destino</option>
            <option v-for="name in citysNames" :key="name.id" :value="name">
              {{ name }}
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
    <div v-if="show" class="container-component">
      <p>
        Estas são as melhores alternativas de frete que encontramos para você.
      </p>
      <div>
        <ItemCard
          :image="imageLowCostTransport"
          :message="msgPrice"
          :leadTime="lowPrice.lead_time"
          :price="valueDelivery"
          :tranpostName="lowPrice.name"
        ></ItemCard>
      </div>
      <div>
        <ItemCard
          :image="imageFastTransport"
          :message="msgFast"
          :leadTime="fastDelivered.lead_time"
          :price="fastDeliveredPrice"
          :tranpostName="fastDelivered.name"
        ></ItemCard>
      </div>
    </div>
    <div class="btn-clear">
      <input
        v-if="show"
        type="button"
        @click="clear(e)"
        class="submit-req-btn-clear"
        value="Limpar"
      />
    </div>
  </div>
</template>



<script>
import ItemCard from "./ItemCard.vue";

export default {
  name: "FormTransportPage",
  components: {
    ItemCard,
  },
  data() {
    return {
      msgPrice: "Frete com o menor valor",
      msgFast: "Frete mais rápido",
      imageFastTransport: require("../assets/fast-time.svg"),
      imageLowCostTransport: require("../assets/money-with-hand.svg"),
      citysNames: null,
      weight: null,
      datas: null,
      city: "",
      lowPrice: null,
      fastDelivered: null,
      fastDeliveredPrice: null,
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

      this.citysNames = Array.from(diffCity).sort();
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
      if (this.weight > 100) {
        this.fastDeliveredPrice = new Intl.NumberFormat("pt-BR", {
          style: "currency",
          currency: "BRL",
        }).format(
          parseFloat(
            this.fastDelivered.cost_transport_heavy.replace(/[^0-9-.]/g, "") *
              this.weight
          )
        );
      } else {
        this.fastDeliveredPrice = new Intl.NumberFormat("pt-BR", {
          style: "currency",
          currency: "BRL",
        }).format(
          (this.fastDeliveredPrice = parseFloat(
            this.fastDelivered.cost_transport_ligth.replace(/[^0-9-.]/g, "") *
              this.weight
          ))
        );
      }
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
  justify-content: space-between;
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
.imagem-map-tranport{
  margin: 10px;
}
.imagem-map-tranport {
  font-size: 5px;
}
.container-component {
  display: flex;
  max-width: 545px;
  flex-direction: column;
  align-items: center;
  height: 100%;
  align-self: center;
  flex: 1;
}
.container-form {
  display: flex;
  padding: 1.5%;
  margin: 3%;
  flex-direction: column;
  max-width: 400px;
  width: 300px;
  justify-content: center;
  align-items: center;
  height: 60vh;
  background-color: #e9ecef;
  border-radius: 8px;
}
.title {
  font-size: 1.2rem;
  text-align: center;
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
.submit-req-btn-clear {
  font-weight: bold;
  width: 60%;
  padding: 10px;
  background-color: #00aca6;
  font-size: small;
}
.input-form-container {
  display: flex;
  flex-direction: column;
}
.btn-clear {
  margin: 5%;
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
