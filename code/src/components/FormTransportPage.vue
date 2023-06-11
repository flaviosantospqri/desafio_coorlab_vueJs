<template>
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
</template>

<script>
export default {
  name: "FormTransportPage",

  data() {
    return {
      citys: null,
      weight: null,
      datas: null,
      city: "",
      lowPrice: null,
      fastDelivered: null,
      listCities: [],
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
      this.findLowPrice();
      this.findFastDelivered();
    },

    findLowPrice() {
      if (this.weight > 100) {
        this.lowPrice = this.listCities.reduce((a, b) => {
          return parseFloat(a.cost_transport_heavy?.replace(/[^0-9-.]/g, "")) <
            parseFloat(b.cost_transport_heavy?.replace(/[^0-9-.]/g, ""))
            ? a
            : b;
        }, 0);
      } else {
        this.lowPrice = this.listCities.reduce((a, b) => {
          return parseFloat(a.cost_transport_light) >
            parseFloat(b.cost_transport_light)
            ? a
            : b;
        }, 0);
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
  },

  created() {
    this.getCitys();
  },
};
</script>

<style scoped>
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
