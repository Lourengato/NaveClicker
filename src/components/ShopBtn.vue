<template>
    <q-card @click="clickEvents" class="cursor-pointer" bg-color="primary">
      <div>
        <img :src="image">
      </div>
      <div>
        {{ title }}
      </div>
      <div>
        {{ price }}
      </div>
      <div>
        {{ quantity }}
      </div>
    </q-card>
</template>

<script>
export default {
  name: 'EssentialLink',
  props: {
    title: {
      type: String,
      required: true
    },

    image:  {
      type: String
    },

    initialPrice: {
      type: Number,
      default: 0
    },

    increaseTax: {
      type: Number,
      default: 0
    },
  },

  data () {
    return {
      price: this.initialPrice,
      quantity: 0
    }
  },

  computed: {
    // getInfo () {
    //   return {price: this.price, quantity: this.quantity}
    // }
  },

  methods: {
    increasePrice () {
      this.price = Math.ceil(this.price * this.increaseTax)
      return this.price
    },

    addQuantity () {
      return this.quantity++
    },

    // saveGame () {
    //   const parsed = JSON.stringify(this.getInfo);
    //   localStorage.setItem('getInfo', parsed);
    // },

    clickEvents () {
      this.addQuantity()
      this.increasePrice()
      // this.saveGame()
    }
  }
}
</script>
