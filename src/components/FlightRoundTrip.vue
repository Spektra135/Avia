<template>
    <div class="flight">
      <div class="flight__flex flight__header">
        <div>
          <span class="logo-carrier"></span>
        </div>

         <div class="flight__price">
           {{ flightPrice }}  ₽
           <p class="flight__price-text">Стоимость для одного взрослого пассажира</p>
         </div>

       </div>

      <FlightsItem class="flight__body" :flight="flight.flight.legs[0]" />

      <div class="first__flight flight__carrier">Рейс выполняет: {{ flight.flight.carrier.caption }}</div>

      <FlightsItem class="flight__body" :flight="flight.flight.legs[1]" />

      <div class="flight__carrier">Рейс выполняет: {{ flight.flight.carrier.caption }}</div>

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