<template>
   <div class="containerForm">
        <h1 class="title">Insira o destino e o peso</h1>
        <form class="form">
            <div class="input-form-container">
                <label for="selectCity">Destino</label>
                <select name="selectCity" id="city" v-model="city">
                    <option value="">Select Option</option>
                    <option>Segunda opção</option>
                </select>
            </div>
           
            <div class="input-form-container">
                <label for="weigth">Peso</label>
                <input type="text" id="weight" name="weigth" value="weigth" v-model="weight">
            </div>
           
            <div class="btn-container">
                <input type="submit" class="submit-req-btn" value="Análisar">
            </div> 
        </form>
   </div>
</template>

<script>
    export default {
        name: "FormTransportPage",

        data () {
        return {
            city: "Curitiba",
            weight: 0.0
        }
    }
    }
   
</script>

<style scoped>
    
    .containerForm{
        display: flex;
        padding: 1.5%;
        margin: 3%;
        flex-direction: column;
        width: 25%;
        justify-content: center;
        align-items: center;
        height: 60vh;
        background-color: #e9ecef ;
        border-radius: 8px;
    }
    .title{
        font-size: 1.6rem;
        margin-bottom: 10%;
    }
    .form{
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        width: 90%;
        height: 50%;
        
    }
    .btn-container{
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .submit-req-btn{
        font-weight: bold;
        width: 60%;
        background-color: #00aca6;
    }
    .input-form-container{
        display: flex;
        flex-direction: column;
    }
    label{
        font-size: small;
        font-weight: bold;
    }
    input, select{
        padding: 10px;
        font-weight: lighter;
        height: 40px;
        border-radius: 4px;
        border: none;
    }
</style>
