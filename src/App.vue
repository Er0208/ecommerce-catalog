<template>
  <ProductDisplay></ProductDisplay>
</template>

<script>

import ProductDisplay from './components/ProductDisplay.vue'

export default {
  name: 'App',
  components: {
    ProductDisplay
  },
  data(){
		return {
			show: true
		}
	},
    mounted(){
        setTimeout(() => {
            this.show = false
        }, 2500)
    }
};

</script>

<style>



</style>
