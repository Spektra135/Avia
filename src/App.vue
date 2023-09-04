<template>
  <main class="content container">
    <div class="content__catalog">
      <FlightFilter :price-from.sync="filterPriceFrom" :price-to.sync="filterPriceTo" :carrier.sync="filterCarrier" :transfer.sync="filterTransfer" :sorting.sync="filterSorting"/>

      <section class="catalog">
        <FlightsList :flights="flights" />
        <button v-if="!IsAllFlights" @click.prevent="showMore">Показать ещё</button>
      </section>
    </div>
  </main>
</template>

<script>
import dataFlights from "@/data/flights.json";
import FlightsList from '@/components/FlightsList.vue'
import FlightFilter from "@/components/FlightFilter";

export default {
  components: {FlightFilter, FlightsList},
  data() {
    return {
      filterPriceFrom: 0,
      filterPriceTo: 0,

      filterFlightTime: 0,
      filterTransfer: [],
      filterCarrier: [],
      filterSorting: '',
      IsAllFlights: false,

      page: 1,
      flightsPerPage: 310,
    }
  },
  computed: {
    filteredFlights() {
      let filteredDataFlights = dataFlights.result.flights;
      console.log(filteredDataFlights);

      if(this.filterPriceFrom > 0) {
        filteredDataFlights = filteredDataFlights.filter(flight => flight.flight.price.total.amount > this.filterPriceFrom)
      }

      if(this.filterPriceTo > 0) {
        filteredDataFlights = filteredDataFlights.filter(flight => flight.flight.price.total.amount < this.filterPriceTo)
      }

      if(this.filterCarrier.length > 0) {
        filteredDataFlights = filteredDataFlights.filter(flight => this.filterCarrier.includes(flight.flight.carrier.caption))
      }
      console.log(filteredDataFlights);
      if(this.filterTransfer.length  > 0) {
        filteredDataFlights = filteredDataFlights.filter(flight => {
          if (this.filterTransfer.length  === 1) {
            return ( (this.filterTransfer).includes(flight.flight.legs[0].segments.length - 1) && (this.filterTransfer).includes(flight.flight.legs[1].segments.length - 1) );
          } else {
            return ( (this.filterTransfer).includes(flight.flight.legs[0].segments.length - 1) || (this.filterTransfer).includes(flight.flight.legs[1].segments.length - 1) );          }
        })
      }

      if(this.filterSorting === 'minPrice') {
        return filteredDataFlights.sort((a, b) => a.flight.price.total.amount - b.flight.price.total.amount);
      } else if(this.filterSorting === 'maxPrice') {
        return filteredDataFlights.sort((a, b) => b.flight.price.total.amount - a.flight.price.total.amount);
      } else if(this.filterSorting === 'minTime') {
        return filteredDataFlights.sort((a, b) => (a.flight.legs[0].duration + a.flight.legs[1].duration) - (b.flight.legs[0].duration + b.flight.legs[1].duration));
      }

      return filteredDataFlights;
    },
    flights() {
      const offset = (this.page - 1) * this.flightsPerPage
      return this.filteredFlights.slice(offset, offset + this.flightsPerPage);
    },
  },

  methods: {
    showMore() {
      this.flightsPerPage = this.flightsPerPage + 2
    }
  }
}
</script>
