<template>
  <form @submit.prevent>
    <h2>Что вы хотите добавить?</h2>
        <my-select @change="whatIsChoose" v-model="typeOfPost" :options="['Машину','Квартиру']"></my-select>
<!--    <input-->
<!--      v-model:="this.typeOfPost"-->
<!--      type="radio"-->
<!--      name="pls"-->
<!--      id="car"-->
<!--      value="car"-->
<!--      :checked="chooseCar"-->
<!--    />-->
<!--    <label for="appartment">Квартиру</label>-->
<!--    <input-->
<!--      v-model:="this.typeOfPost"-->
<!--      type="radio"-->
<!--      name="pls"-->
<!--      id="appartment"-->
<!--      value="apartment"-->
<!--      :checked="chooseApart"-->
<!--    />-->

    <div v-if="this.typeOfPost == 'Машину'">
      <h2>Опубликовать машину</h2>
      <h3>Автомобиль:</h3>
      <div style="display: inline-block; width: 30%; margin-right: 10px">
        <p>Модель автомобиля</p>
        <my-input v-model="objToCreate.model"></my-input>
      </div>
      <div style="display: inline-block; width: 30%; margin-right: 10px">
        <p>Цена автомобиля</p>
        <my-input v-model="objToCreate.price"></my-input>
      </div>
      <div style="display: inline-block; width: 30%; margin-left: 30px">
        <p>Кузов</p>
        <my-select v-model="objToCreate.car_type" :options="optForCarType"></my-select>
      </div>
      <div style="display: inline-block; width: 30%; margin-right: 10px">
        <p>Объём двигателя</p>
        <my-input v-model="objToCreate.engine_volume"></my-input>
      </div>
      <div style="display: inline-block; width: 30%; margin-right: 10px">
        <p>Мощность двигателя</p>
        <my-input v-model="objToCreate.engine_power"></my-input>
      </div>
      <br />
      <h3>Контакты</h3>
      <p style="margin: 1px">Город</p>
            <my-select v-model="objToCreate.city" :options="optForCity"></my-select>
      <p style="margin: 1px">Адрес</p>
      <my-input v-model="objToCreate.address"></my-input>
      <p style="margin: 1px">Телефон</p>
      <my-input v-model="objToCreate.phone"></my-input>

      <my-button @click="this.createCar">Опубликовать</my-button>
    </div>

    <div v-if="this.typeOfPost == 'Квартиру'">
      <h2>Опубликовать квартиру</h2>
      <h3>Квартира:</h3>
      <div style="display: inline-block; width: 30%; margin-right: 10px">
        <p>Комнат</p>
        <my-input v-model="objToCreate.rooms"></my-input>
      </div>
      <div style="display: inline-block; width: 30%; margin-right: 10px">
        <p>Цена</p>
        <my-input v-model="objToCreate.price"></my-input>
      </div>
      <div style="display: inline-block; width: 30%; margin-right: 10px">
        <p>Площадь:</p>
        <my-input v-model="objToCreate.square"></my-input>
      </div>
      <br />

      <h3>Контакты</h3>
      <p style="margin: 1px">Город</p>
      <my-select v-model="objToCreate.city" :options="optForCity"></my-select>
      <p style="margin: 1px">Адрес</p>
      <my-input v-model="objToCreate.address"></my-input>
      <p style="margin: 1px">Телефон</p>
      <my-input v-model="objToCreate.phone"></my-input>
      <my-button @click="this.createApart">Опубликовать</my-button>
    </div>
  </form>
</template>

<script>
import MyInput from "@/components/UI/MyInput";
import MySelect from "../../components/UI/MySelect";
export default {
  name: "CarForm",
  components: { MySelect, MyInput },
  data() {
    return {
      typeOfPost: "",
      objToCreate: {},
      optForCarType: ["Седан", "Внедорожник", "Хетчбек", "Кабриолет"],
      optForCity: [
        "Москва",
        "Санкт-Петербург",
        "Новосибирск",
        "Екатеринбург",
        "Казань",
        "Нижний Новгород",
        "Челябинск",
        "Самара",
        "Омск",
        "Ростов-на-Дону",
        "Уфа",
        "Красноярск",
        "Воронеж",
        "Пермь",
        "Волгоград",
        "Краснодар",
        "Саратов",
        "Тюмень",
        "Тольятти",
        "Ижевск",
      ],
    };
  },
  props: {},
  methods: {
    whatIsChoose(){
      if(this.typeOfPost=='Машину'){
        this.chooseCar();
      }
      else{
        this.chooseApart()
      }
    },
    chooseCar() {
      this.objToCreate = {
        type: "car",
        city: "",
        address: "",
        phone: "",
        price: "",
        model: "",
        car_type: "",
        engine_volume: "",
        engine_power: "",
      };
    },
    createCar() {
      if(this.objToCreate.type==''||
          this.objToCreate.city== ""||
          this.objToCreate.address== ""||
          this.objToCreate.phone== ""||
          this.objToCreate.price== ""||
          this.objToCreate.model== ""||
          this.objToCreate.car_type== ""||
          this.objToCreate.engine_volume== ""||
          this.objToCreate.engine_power== ""){
        alert("ЗАПОЛНИТЕ ВСЕ ПОЛЯ ФОРМЫ!!!");
        return 0;
      }
      else {
        this.$emit("create", this.objToCreate);
        this.objToCreate = {};
      }

    },
    chooseApart() {
      this.objToCreate = {
        type: "apartment",
        city: "",
        address: "",
        phone: "",
        price: "",
        rooms: "",
        square: "",
      };
    },
    createApart() {
      if(this.objToCreate.type==''||
          this.objToCreate.city== ""||
          this.objToCreate.address== ""||
          this.objToCreate.phone== ""||
          this.objToCreate.price== ""||
          this.objToCreate.rooms== ""||
          this.objToCreate.square== ""){
        alert("ЗАПОЛНИТЕ ВСЕ ПОЛЯ ФОРМЫ!!!");
        return 0;
      }
      else {
        this.$emit("create", this.objToCreate);
        this.objToCreate = {};
      }

    },
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  margin-bottom: 30px;
}
</style>
