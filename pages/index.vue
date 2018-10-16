<template>
  <v-data-table
    v-model="selected"
    :headers="headers"
    :items="desserts"
    :pagination.sync="pagination"
    :rows-per-page-items=[10,25,50,100]
    select-all
    item-key="voucherNo"
    class="elevation-1"
  >
    <template slot="headers" slot-scope="props">
      <tr>
        <th>
          <v-checkbox
            :input-value="props.all"
            :indeterminate="props.indeterminate"
            primary
            hide-details
            @click.native="toggleAll"
          ></v-checkbox>
        </th>
        <th
          v-for="header in props.headers"
          :key="header.text"
          :class="['column sortable', pagination.descending ? 'desc' : 'asc', header.value === pagination.sortBy ? 'active' : '']"
          @click="changeSort(header.value)"
        >
          <v-icon small>arrow_upward</v-icon>
          {{ header.text }}
        </th>
      </tr>
    </template>
    <template slot="items" slot-scope="props">
      <tr :active="props.selected" @click="props.selected = !props.selected">
        <td>
          <v-checkbox
            :input-value="props.selected"
            primary
            hide-details
          ></v-checkbox>
        </td>
        <td class="text-xs-center">{{ props.item.no }}</td>
        <td class="text-xs-center">{{ props.item.voucherNo }}</td>
        <td class="text-xs-center">{{ props.item.refNo }}</td>
        <td class="text-xs-center">{{ props.item.bookingDate }}</td>
        <td>{{ props.item.route }}</td>
        <td class="text-xs-center">{{ props.item.travelDate }}</td>
        <td class="text-xs-center">{{ props.item.type }}</td>
      </tr>
    </template>
  </v-data-table>
</template>

<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'

export default {
  components: {
    Logo
  },
  asyncData ({ params, error }) {
    return axios.get(`https://acc.adv-tour.com/api/invoice/list`)
    .then((res) => {
      return { 
        desserts: res.data.data 
      }
    })
    .catch((e) => {
      error(e)
    })
  },
  data: () => ({
      pagination: {
        sortBy: 'voucherNo',
      },
      selected: [],
      headers: [
        { text: 'No', align: 'left', value: 'no' },
        { text: 'VoucherNo', value: 'voucherNo' },
        { text: 'RefNo', value: 'refNo' },
        { text: 'BookingDate', value: 'bookingDate' },
        { text: 'Route', align: 'right', value: 'route' },
        { text: 'TravelDate', value: 'travelDate' },
        { text: 'type', value: 'type'}
      ],
      desserts: []
    }),

    methods: {
      toggleAll () {
        if (this.selected.length) this.selected = []
        else this.selected = this.desserts.slice()
      },
      changeSort (column) {
        if (this.pagination.sortBy === column) {
          this.pagination.descending = !this.pagination.descending
        } else {
          this.pagination.sortBy = column
          this.pagination.descending = false
        }
      }
    }
}
</script>