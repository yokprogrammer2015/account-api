<template>
  <div class="container mx-auto">
    <div class="invoices-container">
      <div v-for="invoice in invoices" :key="invoice.id">
        <div>{{ invoice.no }}</div>
        <div>{{ invoice.voucherNo }}</div>
        <div>{{ invoice.route }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'

axios.defaults.headers.common['user-key'] = '12efacdd543de0cbf0b0f6976f5ea72d'

export default {
  components: {
    Logo
  },
  asyncData ({ params, error }) {
    return axios.get(`https://acc.adv-tour.com/api/invoice/list`)
    .then((res) => {
      return { 
        invoices: res.data.data 
      }
    })
    .catch((e) => {
      error(e)
    })
  },
  data(){
    return {
    }
  }
}
</script>