<template>
  <ul class="flights__list">
    <li class="flights__item" v-for="flight in flights" v-bind:key="flight.flightToken">
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
