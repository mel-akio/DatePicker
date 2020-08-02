<template>
  <div class="component">
    <vue-dropdown class="dropdown" :config="config" @setSelectedOption="updateMonth($event)"></vue-dropdown>
    <div v-if="selectedKey != undefined">
      <div class="calendar">
        <div class="calendar__dayName" v-for="(dayName, i) in daysNames" :key="i" v-on:click="toogleColumn(i)">
          {{dayName}}</div>
        <div v-for="(day, i) in daysOfMonth" :key="'A' + i">
          <div class="calendar__days" v-bind:class="{ 'calendar__days__selected': selectedDays[i] }"
            v-if="day.day && render" v-on:click="toogleDay(i)">
            {{day.day}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import VueDropdown from 'vue-dynamic-dropdown'
  import { Calendar as Calendar } from 'calendar-base'

  export default {
    name: 'DatePicker',
    components: {
      VueDropdown
    },
    data: function () {
      return {
        config: {
          options: [],
          prefix: "Sélectionnez un mois",
          textColor: "#0e171e",
          backgroundColor: "#90caf9",
          width: 315
        },
        calendar: new Calendar({ weekStart: 1 }),
        selectedKey: undefined,
        daysNames: ['L', 'M', 'M', 'J', 'V', 'S', 'D'],
        monthNames: ['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin', 'Juillet', 'Août', 'Septembre', 'Octobre', 'Novembre', 'Décembre'],
        selectedDays: [],
        render: true
      }
    },
    computed: {
      daysOfMonth: function () {
        return this.calendar.getCalendar(this.config.options[this.selectedKey].year, this.config.options[this.selectedKey].month)
      }
    },
    methods: {
      updateMonth: function (event) {
        this.config.prefix = event.value
        this.selectedKey = event.key
        this.selectedDays = []
      },
      toogleDay: function (index) {
        this.selectedDays[index] = !this.selectedDays[index]
        this.render = false
        this.render = true
      },
      toogleColumn: function (index) {
        for (let i = 0; i <= 35; i += 7)
          this.selectedDays[index + i] = !this.selectedDays[index + 35]
        this.render = false
        this.render = true
      },
      getNextMonths: function () {
        const date = new Date()
        let year = date.getFullYear()
        let month = date.getMonth()
        let array = []

        for (let i = 0; i < 12; i++) {
          array.push({
            value: this.monthNames[month - 1] + ' ' + year,
            key: i,
            year: year,
            month: month - 1
          })
          if (month < 12)
            month++
          else {
            month = 1
            year++
          }
        }
        return (array)
      }
    },
    created() {
      this.config.options = this.getNextMonths()
    }
  }
</script>

<style scoped>
  .component {
    width: 296px;
    left: 50%;
    transform: translate(-50%);
    position: absolute;
  }

  .dropdown {
    position: absolute;
    border-radius: 3px;
    z-index: 5;
  }

  .calendar {
    display: grid;
    grid-template-columns: 40px 40px 40px 40px 40px 40px 40px;
    grid-gap: 3px;
    color: #444;
    width: 100%;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
    padding: 10px;
    padding-top: 50px;
    border-radius: 3px;
    background-color: rgb(244, 248, 250);
    -webkit-animation: scale-in-ver-top 0.5s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
    animation: scale-in-ver-top 0.5s cubic-bezier(0.250, 0.460, 0.450, 0.940) both;
    animation-delay: 400ms;
  }

  .calendar__dayName {
    font-weight: 700;
    font-size: 1.1em;
    cursor: pointer;
    color: #636363;
  }

  .calendar__dayName:hover {
    font-weight: 900;
    color: #4d4d4d;
  }

  .calendar__days {
    cursor: pointer;
    padding: 10px;
    font-weight: 600;
    width: 20px;
    height: 22px;
    animation-name: daysRemove;
    animation-duration: 1s;
    animation-fill-mode: forwards;
  }

  .calendar__days__selected {
    animation-name: daysSelected;
    animation-duration: 1s;
    animation-fill-mode: forwards;
  }

  /* Keyframes Animations */

  @keyframes daysSelected {
    0% {
      background-color: #ffffff;
      color: #0e171e;
      border-radius: 3px;
    }

    100% {
      background-color: #5d99c6;
      color: #eef7f7;
      border-radius: 100px;
    }
  }

  @keyframes daysRemove {
    0% {
      background-color: #5d99c6;
      color: #eef7f7;
      border-radius: 100px;
    }

    100% {
      background-color: #ffffff;
      color: #0e171e;
      border-radius: 3px;
    }
  }

  @-webkit-keyframes scale-in-ver-top {
    0% {
      -webkit-transform: scaleY(0);
      transform: scaleY(0);
      -webkit-transform-origin: 100% 0%;
      transform-origin: 100% 0%;
      opacity: 1;
    }

    100% {
      -webkit-transform: scaleY(1);
      transform: scaleY(1);
      -webkit-transform-origin: 100% 0%;
      transform-origin: 100% 0%;
      opacity: 1;
    }
  }

  @keyframes scale-in-ver-top {
    0% {
      -webkit-transform: scaleY(0);
      transform: scaleY(0);
      -webkit-transform-origin: 100% 0%;
      transform-origin: 100% 0%;
      opacity: 1;
    }

    100% {
      -webkit-transform: scaleY(1);
      transform: scaleY(1);
      -webkit-transform-origin: 100% 0%;
      transform-origin: 100% 0%;
      opacity: 1;
    }
  }
</style>