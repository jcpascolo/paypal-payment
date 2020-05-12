<template>
<div class="d-flex justify-content-center">
  <div v-if="payed" class="w-55">
    <h1>Your order have been processed successfully. You will receive your purchase soon.</h1>
  </div>
  <div v-else class="w-55">
    <h1>These are the product your are going to buy</h1>
    <h3>{{paypalDescription}}</h3>
    <p><small>*PayPal Sandbox only works with USD dollars.</small></p>
    <div ref="paypal" class="w-90 my-16"></div>
  </div>

  

</div>
</template>

<script>
export default {
  name: 'PayPalOrder',
  props: ['products', 'paypalCurrency'],
  data() {
    return {
      payed: false,
      loaded: false,
      paypalTotalValue: 0,
      paypalDescription: ""
    }
  },

  methods: {
    setLoaded() {
      this.loaded = true;
      window.paypal.Buttons({
        createOrder: (data, actions) => {
          return actions.order.create({
            purchase_units: [{
              description: this.paypalDescription,
              amount: {
                currency_code: this.paypalCurrency,
                value: this.paypalTotalValue
              }
            }]
          })
        },
        onApprove: async (data, actions) => {
          const order = await actions.order.capture();
          this.payed = true;
          console.log(order);
        },
        onError: err => {
          console.error(err);
        }
      })
      .render(this.$refs.paypal);
    }
  },

  beforeMount() {
    this.products.forEach((product, index) => {
      this.paypalTotalValue += product.price * product.quantity;
      if(index < this.products.length-1){
       this.paypalDescription += product.quantity + " x " + product.name + ",\n";
      }
      else {
        this.paypalDescription += product.quantity + " x " + product.name + ".";
      }
    });
  },

  mounted() {
    console.log(this.paypalCurrency);
    const script = document.createElement("script");
    
    script.src = "https://www.paypal.com/sdk/js?client-id=AeITg6iUZtekHDwoGrntci7oegkV8kxAqLV_vpZnHpewBiZ-e1S9IUIGoL5dn2WHw6R3CpVvaADlB2J9";
    
    script.addEventListener("load", this.setLoaded);
    document.body.appendChild(script);
  }
  
}
</script>


<style scoped>

</style>
