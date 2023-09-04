<template>
  <ul class="catalog__list">
    <li class="catalog__item" v-for="flight in flights" v-bind:key="flight.flightToken">
      <FlightRoundTrip  :flight="flight" />
    </li>
</ul>
</template>

<script>
  import FlightRoundTrip from "@/components/FlightRoundTrip";

  export default {
    props: ['flights'],
    components: {FlightRoundTrip},
  }
</script>
