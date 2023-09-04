<template>
    <div class="flight__container">
      <div class="flight__flex">
        <div>
          <span class="flight__content">{{ departureCity }}, </span>

          <span>{{ departureAirport }} </span>

          <span class="font-blue">({{ departureAirportUid }})</span>
        </div>

        <span class="flight-arrow"></span>

        <div>
          <span>{{ arrivalCity }}, </span>

          <span>{{ arrivalAirport }} </span>

          <span class="font-blue">({{ arrivalAirportUid }})</span>
        </div>
      </div>

      <div class="flight__flex flight-border">
        <div>
          <span>{{ departureTimeFormatted }} </span>

          <span class="font-blue flight__time-day">{{ departureDateFormatted }}</span>

          <span class="font-blue flight__time-day">{{ departureDayFormatted }}</span>
        </div>

        <span class="flight__duration">{{ durationFlightFormatted }} </span>

        <div>
          <span class="font-blue flight__time-day">{{ arrivalDateFormatted }}</span>
          <span class="font-blue flight__time-day">{{ arrivalDayFormatted }} </span>
          <span>{{ arrivalTimeFormatted }} </span>
        </div>
      </div>

      <div v-if="countTransfer > 0"  class="flight__transfer"> {{ countTransfer }} пересадка</div>
    </div>
</template>

<script>

export default {
  props: ['flight'],
  computed: {
    departureCity() {
      return this.flight.segments[0].departureCity.caption
    },
    departureAirport() {
      return this.flight.segments[0].departureAirport.caption
    },
    departureAirportUid() {
      return this.flight.segments[0].departureAirport.uid
    },
    arrivalCity() {
      return this.flight.segments[this.flight.segments.length - 1].arrivalCity.caption
    },
    arrivalAirport() {
      return this.flight.segments[this.flight.segments.length - 1].arrivalAirport.caption
    },
    arrivalAirportUid() {
      return this.flight.segments[this.flight.segments.length - 1].arrivalAirport.uid
    },
    durationFlightFormatted() {
      let duration = this.flight.duration;
      let hours = Math.floor(duration / 60);
      let minutes = (String(duration % 60)).padStart(2, "00");
      return `${hours} ч ${minutes} мин`;
    },
    departureTimeFormatted() {
      let options = {
        hour: 'numeric',
        minute: 'numeric',
        timezone: 'UTC'
      };
      return (new Date(this.flight.segments[0].departureDate)).toLocaleString("ru-RU", options);
    },
    departureDateFormatted() {
      let options = {
        month: 'short',
        day: 'numeric',
        timezone: 'UTC'
      };
      return (new Date(this.flight.segments[0].departureDate)).toLocaleString("ru-RU", options);
    },
    departureDayFormatted() {
      let options = {
        weekday: "short",
        timezone: 'UTC'
      };
      return (new Date(this.flight.segments[0].departureDate)).toLocaleString("ru-RU", options);
    },
    arrivalTimeFormatted() {
      let options = {
        hour: 'numeric',
        minute: 'numeric',
        timezone: 'UTC'
      };
      return (new Date(this.flight.segments[0].arrivalDate)).toLocaleString("ru-RU", options);
    },
    arrivalDateFormatted() {
      let options = {
        month: 'short',
        day: 'numeric',
        timezone: 'UTC'
      };
      return (new Date(this.flight.segments[0].arrivalDate)).toLocaleString("ru-RU", options);
    },
    arrivalDayFormatted() {
      let options = {
        weekday: "short",
        timezone: 'UTC'
      };
      return (new Date(this.flight.segments[0].arrivalDate)).toLocaleString("ru-RU", options);
    },
    countTransfer() {
      return (this.flight.segments.length - 1)
    },
  },
  methods: {
    flights() {
      return result.flights
    },
  }
}
</script>