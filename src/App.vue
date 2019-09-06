<template>
  <div>
    <navbar />
    <div class="container move-down fields-border padding">
      <!-- Probability of heads -->
      <div class="row">
        <div class="col-sm-12">
          <div class="move-right">
            <h3>Probability of Heads</h3>
          </div>
          <div class="move-right">
            <input class="form-control input-size" v-model.number="probabilityOfHeads" />
          </div>
        </div>
      </div>
      <!-- Number of Tosses -->
      <div class="row move-down">
        <div class="col-sm-12">
          <div class="move-right">
            <h3>Number of Tosses</h3>
          </div>
          <div class="move-right">
            <input class="form-control input-size" v-model.number="numberOfTosses" />
          </div>
        </div>
        <div class="row">
          <Coin v-for="n in numberOfTosses" :key="n" />
        </div>
      </div>
      <!-- Number of Repetitions -->
      <div class="row move-down">
        <div class="col-sm-6">
          <div class="move-right">
            <h3>Number of Repetitions</h3>
          </div>
          <div class="move-right">
            <input class="form-control input-size" v-model.number="numberOfRepetitions" />
          </div>
        </div>
        <div class="col-sm-6">
          <h2>Average: {{average}} %</h2>
        </div>
      </div>
      <!-- Calculate button -->
      <div class="move-down">
        <button class="btn btn-success" @click="calculateAverage">Calculate</button>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar'
import Coin from './components/Coin'

export default {
  data() {
    return {
      probabilityOfHeads: 0.5,
      numberOfTosses: 10,
      numberOfRepetitions: 1000,
      heads: 0,
      average: 0
    }
  },
  methods: {
    calculateAverage() {
      // Either 1 or 0
      const generateOneOrZero = () => {
        return Math.round(Math.random())
      }

      const oneSetOfTosses = () => {
        // make sure heads is reset to 0 every time
        this.heads = 0
        for (let i = 0; i < this.numberOfTosses; i++) {
          if (generateOneOrZero() === 0) {
            this.heads++
          }
        }
        return this.heads / this.numberOfTosses
      }

      const performAllSetsOfCoinTosses = () => {
        let totalCoinTossRatio = 0
        for (let i = 0; i < this.numberOfRepetitions; i++) {
          totalCoinTossRatio += oneSetOfTosses()
        }
        return parseFloat(
          totalCoinTossRatio / this.numberOfRepetitions
        ).toFixed(2)
      }

      this.average = performAllSetsOfCoinTosses()
    }
  },
  components: {
    Navbar,
    Coin
  }
}
</script>

<style>
body {
  background: #f5f5f5;
}

.move-down {
  margin-top: 2em;
}

.move-right {
  margin-right: 0.8em;
}

.input-size {
  width: 75px;
}

.fields-border {
  border: 3px solid black;
}

.padding {
  padding: 1em;
}
</style>
