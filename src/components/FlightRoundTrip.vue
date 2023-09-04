<template>
    <div class="flight">
       <div class="flight__flex flight__header">
          Logo
         <div class="flight__price">
           {{ flightPrice }}  ₽
           <p class="flight__price-text">Стоимость для одного взрослого пассажира</p>
         </div>

       </div>
      <FlightsItem class="flight__body" :flight="flight.flight.legs[0]" />
      <FlightsItem class="flight__body" :flight="flight.flight.legs[1]" />
      <div>Рейс выполняется: {{ flight.flight.carrier.caption }}</div>
      <button class="flight__button">ВЫБРАТЬ</button>
    </div>
</template>

<script>
import FlightsItem from "@/components/FlightItem";

export default {
  components: {FlightsItem},
  props: ['flight'],
  computed: {
    flightPrice() {
      return this.flight.flight.price.passengerPrices[0].total.amount;
    },
  }
}
</script>