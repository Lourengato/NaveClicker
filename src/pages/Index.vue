<template>
  <div class="row items-center justify-between q-pa-xl">
    <div class="column items-center q-gutter-lg">
      <div class="text-center">
        <div class="text-h4  q-mb-lg">Poeira espacial: {{ spaceDust }}</div>
        <div class="q-mb-lg">Poeira espacial por segundo: {{ multiply }}</div>
        <div class="q-mb-lg">Poeira espacial por clique {{ cursorMultiply }}</div>
      </div>

      <q-btn round flat @click="addSpaceDust">
        <img src="../assets/rocket.svg" style="width: 350px;">
      </q-btn>
      <q-btn label="Resetar" @click="resetData" />
    </div>
    
    <div style="width: 500px;" class="q-gutter-lg">
      <div v-for="(item, index) in shops" :key="index" class="column">
        <q-card @click="addItem(item)" class="cursor-pointer q-pa-md relative-position" bg-color="primary">
          <div class="text-subtitle2">{{ item.title }}</div>
          <div>{{ item.price }}</div>
          <div class="absolute-right text-h3 q-pt-md q-pr-md">{{ item.quantity }}</div>
        </q-card>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      spaceDust: 0,
      multiply: 0,
      cursorMultiply: 1,
      shops: {
        helpingHand: {
          name: 'helpingHand',
          price: 50,
          increaseTax: 1.50,
          itemMultiply: 1,
          title: 'Mão mecanica amiga',
          quantity: 0,
          isCursor: true
        },
        smallAsteroid: {
          name: 'smallAsteroid',
          price: 100,
          increaseTax: 1.50,
          itemMultiply: 1.50,
          title: 'Meteoro pequeno',
          quantity: 0,
          isCursor: false
        },
        smallSatelite: {
          name: 'smallSatelite',
          price: 500,
          increaseTax: 1.50,
          itemMultiply: 1.80,
          title: 'Satélite pequeno',
          quantity: 0,
          isCursor: false
        }
      }
    }
  },

  mounted () {
    if (localStorage.getItem('spaceDust')) {
        try {
          this.spaceDust = JSON.parse(localStorage.getItem('spaceDust'))
        } catch (error) {
          localStorage.removeItem('spaceDust');
        }
      }

      if (localStorage.getItem('cursorMultiply')) {
        try {
          this.cursorMultiply = JSON.parse(localStorage.getItem('cursorMultiply'))
        } catch (error) {
          localStorage.removeItem('cursorMultiply');
        }
      }

      if (localStorage.getItem('multiply')) {
        try {
          this.multiply = JSON.parse(localStorage.getItem('multiply'))
        } catch (error) {
          localStorage.removeItem('multiply');
        }
      }

    for (var item in this.shops){
      this.retriveItems(this.shops[item])
    }
    this.spaceDustTimer()
  },

  methods: {
    addSpaceDust () {
      this.spaceDust = Math.floor(this.spaceDust + (this.cursorMultiply * 1))
      this.saveGame()
    },

    addItem (item) {
      if (this.spaceDust >= item.price) {
        this.spaceDust = this.spaceDust - item.price
        item.quantity++
        item.price = Math.floor(item.price * item.increaseTax)
        if (item.isCursor) {
          this.cursorMultiply =  Math.floor(this.cursorMultiply + item.itemMultiply)
          this.saveItems(item)
        } else {
          this.multiply = Math.floor(this.multiply + item.itemMultiply)
          this.saveItems(item)
        }
        this.saveGame()
      }
    },

    saveItems (item) {
      const price = JSON.stringify(item.price)
      const quantity = JSON.stringify(item.quantity)
      localStorage.setItem(item.name + 'Price', price)
      localStorage.setItem(item.name + 'Quantity', quantity)
    },

    retriveItems (item) {
      if (localStorage.getItem(item.name + 'Quantity') && localStorage.getItem(item.name + 'Price')) {
        try {
          item.quantity = JSON.parse(localStorage.getItem(item.name + 'Quantity'))
          item.price = JSON.parse(localStorage.getItem(item.name + 'Price'))
        } catch (error) {
          localStorage.removeItem(item.name + 'Price')
          localStorage.removeItem(item.name + 'Quantity')
        }
      }
    },

    // retriveData (item) {
    //   if (localStorage.getItem(item)) {
    //     try {
    //       item.quantity = JSON.parse(item)
    //     } catch (error) {
    //       localStorage.removeItem(item);
    //     }
    //   }
    // },

    spaceDustTimer () {
      setTimeout(() => {
        this.spaceDust = Math.floor(this.spaceDust + this.multiply)
        this.spaceDustTimer()
        this.saveGame()
      }, 1000)
    },

    saveGame () {
      const spaceDust = JSON.stringify(this.spaceDust)
      localStorage.setItem('spaceDust', spaceDust)
      const cursorMultiply = JSON.stringify(this.cursorMultiply)
      localStorage.setItem('cursorMultiply', cursorMultiply)
      const multiply = JSON.stringify(this.multiply)
      localStorage.setItem('multiply', multiply)
    },

    resetData () {
      this.spaceDust = 0
      this.multiply = 0
      this.cursorMultiply = 1
      localStorage.clear()
      this.$forceUpdate()
    }
  }
}
</script>
