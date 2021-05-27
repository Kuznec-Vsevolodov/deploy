<template>
  <div>
    <h2 align="center">Зарегистрируйтесь</h2>
    <b-container class="reg-container">
    <div class="register-form">
      <input class="text-fields" v-model="name" type="text" placeholder="Ваше имя">
      <input class="text-fields" v-model="email" type="email" placeholder="Ваша почта">
      <input class="text-fields" v-model="password" type="password" placeholder="Ваш пароль">]
      <button @click="sendData">Зарегистрироваться</button>
      <p style="color: #fff" v-if="isVisible">{{warning}}</p>
    </div>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data(){
    return{
      is_owner: false,
      password: '',
      email: '',
      name: '',
      isVisible: false,
      warning: ''
    }
  },
  methods:{
   sendData(){
     if(this.password == '' || this.email == 0 || this.name == ''){
       this.warning = 'Вы не можете зарегистрироваться. Данные неверны'
       this.isVisible = true
     }else{
       axios.post('http://127.0.0.1:8000/demo/register/', {
          "username": this.name,
          "password": this.password,
          "email": this.email,
          "is_owner": this.is_owner
        })
        .then((response) => {
          console.log(response)
          localStorage.userId = response.data.id;
          axios.post('http://127.0.0.1:8000/demo/token/', {
          "username": this.name,
          "password": this.password
        })
          .then((response) => {
            console.log(response.data);
            localStorage.accessToken = response.data.access;
            localStorage.refreshToken = response.data.refresh;
            localStorage.name = this.name;
            console.log(localStorage.accessToken)
            setTimeout(() => {
              location.href = '/';
            }, 1000);
          })
        })
        .catch((error) => {
          console.log(error)
          this.warning = 'Произошла ошибка, возможно, уже есть пользователь с данным именем'
          this.isVisible = true
        });
     }
   }
  }
}
</script>

<style scoped>
  .register-form{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    width: 500px;
    padding-left: 20px;
    padding-right: 20px;
    padding-top: 20px;
    padding-bottom: 20px;
    background-color: cadetblue;
    border-radius: 15px;
  }
  .text-fields{
    width: 100%;
    border: none;
    border-radius: 50px;
    height: 35px;
    margin-bottom: 10px;
    padding-left: 10px;
  }
  label{
    color: #fff;
    margin-bottom: 0px;
  }
  .owner-detector{
    width: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
  }
  .reg-container{
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }
  button{
    border: none;
    background: #fff;
    color: cadetblue;
    border-radius: 50px;
    font-weight: bold;
    height: 30px;
    line-height: 5px;
    margin-top: 10px;
  }
</style>