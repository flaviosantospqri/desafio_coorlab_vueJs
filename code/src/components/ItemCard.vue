<template>
    <div class="container-card" :v-model="modelValue">
        <div class="top-message">
            <p>Estas são as melhores alternativas de frete que encontramos para você</p>
        </div>
        <div class="container-informations">
            <section class="data-transport">
                <h4>{{message}}</h4>
                <div class="figure">
                    <span>vai entrar uma imagem</span>
                </div>
               <p>Transportadora: <span>{{tranpostName}}</span></p>
               <p>Tempo Estimando: <span>{{leadTime}}</span></p>
            </section>
            <section class="data-value">
                <div class="figure">
                    <span>vai entrar uma imagem</span>
                </div>
                <p> Preço: <span>{{price}}</span></p>
            </section>
        </div>
    </div>
   
</template>

<script>
    export default {
        name: "ItemCard",

        props: {
            message:{
                type: String,
                required: true
            }, 
            tranpostName:{
                type: String,
                required: true
            },
            leadTime:{
                type: String,
                required: true
            },
            price:{
                type: String,
                required: true
            }
           
        },
    }
</script>

<style>
    .container-informations{
        width: 100%;
        padding: 5px;
        display: none;
        justify-content: space-between;
    }
    .data-transport{
        padding: 10px;
        border-radius: 4px;
        display: flex;
        flex-direction: column;
        align-content: center;
        justify-content: center;
        flex: 1;
        background-color: #CCCCCC ;
    }
    .data-value{
        display: flex;
        border-radius: 4px;
        align-items: center;
        justify-content: center;
        background-color: #CCCCCC ;
        padding: 10px;
        width: 30%;
        margin-left: 10px;
    }
    .top-message{
        display: none;
        text-align: left;
        width: 80%;
        color: #6C757D;
        font-size: medium;
        font-weight: lighter;
    }
</style>