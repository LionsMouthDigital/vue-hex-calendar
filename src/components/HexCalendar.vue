<template>
  <div class="calendar">
    <header class="calendar-header">
      <div class="controls" v-if="controls">
        <div v-if="controls === true || controls === 'year'">
          <button class="button-prev" @click.prevent="prevYear()">
            <span class="sr-only">Previous year</span>
          </button>
          <span class="active-date">{{ moment(date).format('YYYY') }}</span>
          <button class="button-next" @click.prevent="nextYear()">
            <span class="sr-only">Next year</span>
          </button>
        </div>

        <div v-if="controls === true || controls === 'month'">
          <button class="button-prev" @click.prevent="prevMonth()">
            <span class="sr-only">Previous month</span>
          </button>
          <span class="active-date">{{ moment(date).format('MMM') }}</span>
          <button class="button-next" @click.prevent="nextMonth()">
            <span class="sr-only">Next month</span>
          </button>
        </div>
      </div>

      <div class="days-of-week">
        <div v-for="day in ['S', 'M', 'T', 'W', 'Th', 'F', 'Sa']">{{ day }}</div>
      </div>
    </header>

    <div class="week" v-for="week in weeks">
      <div class="day" v-for="day in week">{{ moment(day, format).format('DD') }}</div>
    </div>
  </div>
</template>




<script>
  import moment from 'moment';

  export default {
    name: 'HexCalendar',


    props: {
      /**
       * Specify controls to show.
       *
       * Acceptable values include:
       *  - `true` for all,
       *  - `false` for none,
       *  - `month` for only the month, or
       *  - `year` for only the year (for some reason...).
       *
       * @type {Object}
       */
      controls: {
        type:    [Boolean, String],
        default: true,
      },
    },


    data() {
      return {
        date:   moment().format('YYYY-MM-DD'),
        format: 'YYYY-MM-DD',
        moment: moment,
      };
    },


    computed: {
      /**
       * Build an array of moments for each day in the month of `this.date`.
       *
       * The start and end are padded with days to fill the calendar page when the month doesn't
       * start on a Sunday or end on a Saturday.
       *
       * @author Curtis Blackwell
       * @return {array}
       */
      days() {
        // Get the number of days in the set month (`this.month`).
        var daysInMonth = moment(this.date, this.format).daysInMonth();
        var month       = moment(this.date, this.format).format('YYYY-MM');
        var date;

        // Add each day in the month to an array.
        var days = [];
        for (var i = 1; i <= daysInMonth; i++) {
          date = i < 10 ? '0' + i : i;
          days.push(moment([month, date].join('-'), this.format).format(this.format));
        }

        // Pad the month if necessary.
        var monthStart = days[0];

        // If the first day of the month isn't a Sunday, prepend each previous day until
        // Sunday, inclusively.
        if (moment(monthStart, this.format).day() > 0) {
          var subtrahend;
          for (var i = 0; i < moment(monthStart, this.format).day(); i++) {
            subtrahend = i + 1;
            days.unshift(
              moment(monthStart, this.format).subtract(subtrahend, 'day').format(this.format)
            );
          }
        }

        // If the last day of the month isn't Saturday, add each next day until Saturday,
        // inclusively. Calculate by making sure the total number of days is evenly divisible by 7.
        var lastDay;
        while ((days.length / 7) % 1 > 0) {
          lastDay = days[days.length - 1];
          days.push(moment(lastDay, this.format).add(1, 'day').format(this.format));
        }

        return days;
      },

      /**
       * Split the days up into weeks so the template can iterate over them to properly create rows.
       *
       * `<tr v-if="...">` doesn't work, because the tag closes before anything can be entered in.
       *
       * @author Curtis Blackwell
       * @return {array}
       */
      weeks() {
        var totalWeeks = this.days.length / 7;

        var weeks = [];
        for (var i = 0; i < totalWeeks; i++) {
          weeks.push(this.days.slice(i * 7, i * 7 + 7));
        }

        return weeks;
      },
    },


    methods: {
      /**
       * Go to the next month.
       *
       * @author Curtis Blackwell
       * @return {void}
       */
      nextMonth() {
        this.date = moment(this.date, this.format).add(1, 'month').format(this.format);
      },

      /**
       * Go to the previous month.
       *
       * @author Curtis Blackwell
       * @return {void}
       */
      prevMonth() {
        this.date = moment(this.date, this.format).subtract(1, 'month').format(this.format);
      },

      /**
       * Go to the next year.
       *
       * @author Curtis Blackwell
       * @return {void}
       */
      nextYear() {
        this.date = moment(this.date, this.format).add(1, 'year').format(this.format);
      },

      /**
       * Go to the previous year.
       *
       * @author Curtis Blackwell
       * @return {void}
       */
      prevYear() {
        this.date = moment(this.date, this.format).subtract(1, 'year').format(this.format);
      },
    },
  }
</script>
