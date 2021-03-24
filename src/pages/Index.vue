<template>
  <div>
    <div>Poeira espacial: {{ spaceDust }}</div>
    <q-btn round flat @click="addSpaceDust">
        <img src="../assets/rocket.svg">
    </q-btn>
    <div v-for="(item, index) in shops" :key="index">
      <shop-btn :initial-price="item.initialPrice" :increase-tax="item.increaseTax" :title="item.title" />
    </div>
  </div>
</template>

<script>
import ShopBtn from '../components/ShopBtn.vue'

export default {
  components: {
    ShopBtn
  },

  data () {
    return {
      spaceDust: 0,
      shops: {
        intern: {
          initialPrice: 1,
          increaseTax: 1.50,
          title: 'Estagiários'
        },
        juniorDeveloper: {
          initialPrice: 5,
          increaseTax: 1.50,
          title: 'Dev junior'
        }
      }
    }
  },

  mounted () {
    if (localStorage.getItem('spaceDust')) {
      try {
        this.spaceDust = JSON.parse(localStorage.getItem('spaceDust'));
      } catch (error) {
        localStorage.removeItem('spaceDust');
      }
    }
  },

  methods: {
    addSpaceDust () {
      this.spaceDust++
      this.saveGame();
    },

    saveGame () {
      const parsed = JSON.stringify(this.spaceDust);
      localStorage.setItem('spaceDust', parsed);
    }
  }
}
</script>
