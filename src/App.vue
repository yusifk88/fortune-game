<template>
  <FortuneWheel
      style="width: 500px;"
      :canvas="canvasOptions"
      :prizes="prizes"
      :verify="cavansVerify"
      @rotateStart="onCanvasRotateStart"
      @rotateEnd="onRotateEnd"
  />

</template>

<script>
import FortuneWheel from 'vue-fortune-wheel'
import 'vue-fortune-wheel/style.css'
export default {
  name: 'App',
  components: {
    FortuneWheel

  },

  data() {
    return {
      cavansVerify: true, // Whether the turntable in canvas mode is enabled for verification
      canvasOptions: {
        borderWidth: 0.5,
        borderColor: '#ffe700'
      },
      prizes: [
        {
          id: 1, //* The unique id of each prize, an integer greater than 0
          name: 'Free 1 month \nSubscription', // Prize name, display value when type is canvas (this parameter is not needed when type is image)
          value: 'Free 1 month Subscription', //* Prize value, return value after spinning
          bgColor: '#42d70d', // Background color (no need for this parameter when type is image)
          color: '#000000', // Font color (this parameter is not required when type is image)
          probability: 20, //* Probability, up to 4 decimal places (the sum of the probabilities of all prizes
          weight: 1 // Weight, if useWeight is true, the probability is calculated by weight (weight must be an integer), so probability is invalid
        },
        {
          id: 2,
          name: 'GHS100 \n in Wallet',
          value: 'GHS1000',
          bgColor: '#3f057e',
          color: '#ffffff',
          probability: 5,
          weight: 0.5
        },
        {
          id: 3,
          name: 'Built \nT-Shirt',
          value: 'Shirt',
          bgColor: '#fef151',
          color: '#000000',
          probability: 10,
          weight: 1
        },
        {
          id: 4,
          name: 'Branded \n Umbrella',
          value: 'Umbrella',
          bgColor: '#032f83',
          color: 'white',
          probability: 10,
          weight: 1
        },
   {
          id: 5,
          name: 'Free \nInsurance',
          value: 'Free keyman insurance',
          bgColor: '#ff8d00',
          color: 'black',
          probability: 20,
          weight: 1
        },
  {
          id: 6,
          name: 'No Luck',
          value: 'Nothing',
          bgColor: '#000000',
          color: 'white',
          probability: 35,
          weight: 1
        }


      ]
    }
  },
  methods: {
    onImageRotateStart() {
      console.log('onRotateStart')
    },
    onCanvasRotateStart(rotate) {
      rotate();

      if (this.canvasVerify) {
        const verified = true // true: the test passed the verification, false: the test failed the verification
        this.DoServiceVerify(verified, 1000)
            .then((verifiedRes) => {
          if (verifiedRes) {
            console.log('Verification passed, start to rotate')
            rotate() // Call the callback, start spinning
            this.canvasVerify = false // Turn off verification mode
          } else {
            alert('Failed verification')
          }
        })
        return
      }
      console.log('onCanvasRotateStart')
    },
    onRotateEnd(prize) {
      alert(prize.value)
    },
    // Simulate the request back-end interface, verified: whether to pass the verification, duration: delay time
    DoServiceVerify(verified, duration) {
      return new Promise((resolve) => {
        setTimeout(() => {
          resolve(verified)
        }, duration)
      })
    }


  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
