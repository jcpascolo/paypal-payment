<template>
<div class="card">
  <img :src="photo" alt="Avatar">
  <div class="card-quantity d-flex justify-content-center align-items-center">
      <span>{{quantity}}</span>
  </div>
  <div class="card-body">
    <h4><b>{{name.toUpperCase()}}</b></h4>
    <p>{{price}} $</p>
  </div>
  <div class="d-flex justify-content-around my-8">
      <button class="w-45 addOne" @click="addOne()">Add one</button>
      <button class="w-45 removeOne" @click="removeOne()">Remove one</button>
  </div>
</div>
</template>

<script>
export default {
    name: 'ProductCard',
    props: ['name', 'price', 'photo'],
    data() {
        return {
            quantity: 0
        }
    },
    methods: {
        addOne() {
            if(this.quantity < 100){
                this.quantity++;
                this.$emit('addOne', this.quantity);
            }
        },
        removeOne() {
            if(this.quantity > 0){
                this.quantity--;
                this.$emit('removeOne', this.quantity);
            }
        }
    }
}
</script>

<style scoped>
.card {
    position: relative;
  border-radius: 4px;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
}

.card img {
    width: 100%;
}

.card-quantity {
    height: 20px;
    width: 20px;
    position: absolute;
    top: 5px;
    right: 0px;
    background-color: #4CAF50;
    color: white;
}

.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
}

.card-body {
  padding: 2px 16px;
}

.addOne {
    background: #4CAF50;
    color: white;
    border: 2px solid #4CAF50;
    border-radius: 4px;
}

.removeOne {
    background: red;
    color: white;
    border: 2px solid red;
    border-radius: 4px;
}
</style>