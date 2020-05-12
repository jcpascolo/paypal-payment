<template>
  <div id="app">
    <header class="d-flex justify-content-between align-items-center px-16 py-8">
      <p>PayPal Store</p>
      <div>        
        <button class="d-flex justify-content-between align-items-center buy-button py-8 px-8" @click="productPage = !productPage">
          {{productPage ? 'Pay the products' : 'Return to products'}}
          <div class="d-flex justify-content-center align-items-center product-quantity">
            <p class="my-0">
              {{productsToBuy.length}}
            </p> 
          </div>
        </button>
      </div>
      <select v-model="selectedCurrency" class="buy-button py-8 px-8 mx-8">
        <option disabled value="">Seleccione la moneda</option>
        <option>USD</option>
        <option>EUR</option>
      </select>    
    </header>
    <div v-if="productPage" class="d-flex justify-content-around header-footer-space">
      <ProductCard class="w-xs-90 w-md-45 w-30 my-16" 
        v-for="product in availableProducts" :key="product.id" 
        :name="product.name"
        :price="product.price"
        :photo="product.photo"
        @addOne="addOne($event, product.id, product.name, product.price)"
        @removeOne="removeOne($event, product.id)"
      />
    </div>
    <PayPalOrder class="header-footer-space" v-else :products="productsToBuy" :paypalCurrency="selectedCurrency"/>
    <footer class="px-16 py-8">
      Product made by: Juan Carlos González Pascolo
    </footer>
  </div>
</template>

<script>
import PayPalOrder from './components/PayPalOrder.vue';
import ProductCard from './components/ProductCard.vue';

export default {
  name: 'App',
  components: {
    PayPalOrder,
    ProductCard
  },
  data() {
    return {
      productPage: true,
      selectedCurrency: "USD",
      availableProducts: [
        {
          id: 1,
          name: "Nike air force 1 low scribble",
          price: 110.95,
          photo: "https://www.courir.es/dw/image/v2/BCCL_PRD/on/demandware.static/-/Sites-master-catalog-courir/default/dwcfa96aec/images/hi-res/001473840_101.png?sw=600&sh=600&sm=fit"
        },
        {
          id: 2,
          name: "Nike air force 1 low",
          price: 99.99,
          photo: "https://www.courir.es/dw/image/v2/BCCL_PRD/on/demandware.static/-/Sites-master-catalog-courir/default/dw651cd3ba/images/hi-res/001462256_101.png?sw=600&sh=600&sm=fit"
        },
        {
          id: 3,
          name: "Nike air force 1 low fk",
          price: 100.95,
          photo: "https://www.courir.es/dw/image/v2/BCCL_PRD/on/demandware.static/-/Sites-master-catalog-courir/default/dw0cc2f6c4/images/hi-res/001445345_101.png?sw=600&sh=600&sm=fit"
        },
        {
          id: 4,
          name: "Nike air force 1 low overbranding",
          price: 89.99,
          photo: "https://www.courir.es/dw/image/v2/BCCL_PRD/on/demandware.static/-/Sites-master-catalog-courir/default/dw514704c6/images/hi-res/001473842_101.png?sw=600&sh=600&sm=fit"
        },
      ],
      productsToBuy: [ ]
    }
  },

  methods: {
    addOne($event, id, name, price) {
      let isNotIncluded = true;
      let newProductsToBuy = this.productsToBuy.map(product => {
        if(product.id == id) {
          isNotIncluded = false;
          return { 
            id: product.id,
            name: product.name,
            price: product.price,
            quantity: $event 
          };
        }
        else {
          return product;
        }
      });

      if(isNotIncluded){
        newProductsToBuy = [...newProductsToBuy, {
          id: id,
          name: name,
          price: price,
          quantity: 1
        }]
      }      
      this.productsToBuy = newProductsToBuy;
    },

    removeOne($event, id){
      let newProductsToBuy = [];
      if($event === 0){
        newProductsToBuy = this.productsToBuy.filter(product => {
          if(product.id !== id){
            return true;
          }
          else {
            return false;
          }
        })
      }
      else {
        newProductsToBuy = this.productsToBuy.map(product => {
          if(product.id == id) {
            return { 
              id: product.id,
              name: product.name,
              price: product.price,
              quantity: $event 
            };
          }
          else {
            return product;
          }
        });
      }
      this.productsToBuy = newProductsToBuy;
    }
  }
}
</script>

<style>
.d-flex {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.justify-content-center {
  justify-content: center;
}

.justify-content-between {
  justify-content: space-between;
}

.justify-content-around {
  justify-content: space-around;
}

.align-items-center {
  align-items: center;
}

.w-30 {
  width: 30%;
}

.w-45 {
  width: 45%;
}

.w-55{
  width: 55%;
}

.w-90 {
  width: 90%;
}

.mx-0 {
  margin-left: 0px;
  margin-right: 0px;
}

.my-0 {
  margin-top: 0px;
  margin-bottom: 0px;
}

.mx-8 {
  margin-left: 8px;
  margin-right: 8px;
}

.mx-16 {
  margin-left: 16px;
  margin-right: 16px;
}

.my-8 {
  margin-top: 8px;
  margin-bottom: 8px;
}

.my-16 {
  margin-top: 16px;
  margin-bottom: 16px;
}

.px-8 {
  padding-left: 8px;
  padding-right: 8px;
}

.px-16 {
  padding-left: 16px;
  padding-right: 16px;
}

.py-8 {
  padding-top: 8px;
  padding-bottom: 8px;
}

.py-16 {
  padding-top: 16px;
  padding-bottom: 16px;
}

header {
  position: fixed;
  box-sizing: border-box;
  z-index: 10;
  width: 100%;
  top:0;
  left: 0;
  background: rgb(41, 36, 36);
  color: white
}

.header-footer-space {
  margin-top: 65px;
  margin-bottom: 35px;
}

footer {
  position: fixed;
  box-sizing: border-box;
  z-index: 10;
  width: 100%;
  bottom: 0;
  left: 0;
  background: rgb(41, 36, 36);
  color: white;
}

.buy-button {
  width: 164px;
  background-color: rgba(76, 175, 79, 1);
  color: white;
  border: 2px solid #4CAF50; 
  border-radius: 4px;
}

.buy-button:hover {
  background-color: rgba(76, 175, 79, 0.9);
  color: white;
}

.product-quantity {
  width: 20px;
  height: 20px;
  background-color: green;
  color: white;
  border-radius: 50%;
}

@media (max-width: 991.98px){
  .w-md-30 {
    width: 30%;
  }

  .w-md-45 {
    width: 45%;
  }

  .w-md-90 {
    width: 90%;
  }
}

@media (max-width: 787.98px){
  .w-xs-30 {
    width: 30%;
  }

  .w-xs-45 {
    width: 45%;
  }

  .w-xs-90 {
    width: 90%;
  }
}
</style>
