<template>
<div>
  <h3 align="center">Добавить позицию</h3>
    <b-container class="input-info-group">
        <input type="text" v-model="title" :placeholder="place_data.title">
        <input type="text" v-model="address" :placeholder="place_data.address">
        <label for="file">Загрузить изображение</label>
        <input type="file" id="file" ref="image" v-on:change="fileUpload()" style="visibility:hidden;">
        <button @click="sendData">Отправить данные</button>
    </b-container>
</div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      place_data: [],
      image: '',
      title: '',
      address: '',
    }
  },
  methods: {
    fileUpload() {
      this.file = this.$refs.image.files[0];
    },
    sendData() {
      let fd = new FormData()
        fd.append('title', this.title)
        fd.append('general_scheme', this.image)
        fd.append('address',this.address)
        fd.append('owner', localStorage.userId)

      axios.post('http://192.168.220.14:8000/demo/red-places/', fd,
          {headers: {Authorization: `Bearer ${localStorage.accessToken}`}})
          .then((response) => {
            this.place_data = response.data
            this.title = response.data.title
            this.address = response.data.address
            this.image = response.data.general_scheme
          })
    }
  }
}
</script>

<style scoped>

.input-info-group{
  display: flex;
  flex-direction: column;
  margin-top: 50px;
}
.input-info-group input{
  border: 2px solid #CCCCCC;
  border-radius: 50px;
  height: 35px;
  margin-bottom: 15px;
  padding-left: 10px;
}
.input-info-group button{
  border: 2px solid #000;
  background: #fff;
  border-radius: 50px;
  height: 35px;
  color: #000000;
  font-weight: bold;
}
</style>