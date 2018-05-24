
<template>
  <div id="app">
      <p v-if="isLoading">Loading!</p>
      <h2>Shopping Cart - {{ this.items.length }}</h2>
      <p v-if="items.length == 0">No items!</p>
      <p v-for="(item, index) in items"><img height="128px" width="128px" :key="index" :src="item.productimageurl" alt=""> {{ item.productname }} <button class="save" @click="savedUser(index)">Save for later</button> <button class="delete" @click="deleteUser(index)">Delete</button> </p>      
      <h2>Saved Items - {{ this.savedItems.length }}</h2>
      <p v-if="savedItems.length == 0">No Saved Items!</p>
      <p v-for="(item, index) in savedItems" ><img height="128px" width="128px" :key="index" :src="item.productimageurl" alt=""> {{ item.productname }} <button class="move" @click="moveToCard(index)">Move to card</button> <button class="delete" @click="deleteSavedUser(index)">Delete</button></p>
  </div>
</template>



<script>
import axios from 'axios'

export default {
  name: 'app',
  data () {
    return {
      isLoading: true,
      items : [],
      errors: [],
      savedItems : []
    }
  },
  created() {
    axios.get('../data2.json')
    .then(res => {
        console.log(res.data);
        this.items = res.data;
        this.isLoading = false;
    })
    .catch(e => {
      this.errors.push(e)
    })
  },
  methods: {
    deleteUser(index){
      this.items.splice(index, 1);
    },
    deleteSavedUser(index){
      this.savedItems.splice(index, 1);
    },
    savedUser(index){
      const item = this.items.splice(index, 1);
      this.savedItems.push(item[0]);
    },
    moveToCard(index){
      const item = this.savedItems.splice(index, 1);
      this.items.push(item[0]);
    }
  }
}
</script>

<style>
#app {
  font-family: 'Comfortaa';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 0 10px;
}

a {
  color: #42b983;
}
p{
  padding:50px;
  border:1px solid #DDD;
  border-radius: 3px;
  box-shadow: 2px 0 2px #EEE;
  width:800px;
  margin:auto;
  margin-top:5px;
  position: relative;
  text-align: center;
  position: relative;
  font-weight: bold;
}
img{
  position: absolute;
  left:50px;
  top:5px;
}
.move{
  padding: 15px;
  border:none;
  outline: none;
  font:12px 'Comfortaa';
  font-weight: bold;
  border-radius: 3px;
  background: #069;
  color:#FFF;
  cursor: pointer;
}
.delete{
  padding: 15px;
  border:none;
  outline: none;
  font:12px 'Comfortaa';
  font-weight: bold;
  border-radius: 3px;
  background: #e74c3c;
  color:#FFF;
  cursor: pointer;  
}
.save{
  padding: 15px;
  border:none;
  outline: none;
  font:12px 'Comfortaa';
  font-weight: bold;
  border-radius: 3px;
  background:#2ecc71;
  color:#FFF;
  cursor: pointer;
}
</style>
