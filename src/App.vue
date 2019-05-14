<template>
  <div class="container-fluid wrapper">
    <pre>{{ currentView }}</pre>
    <div class="row top-panel">
      <div class="col-3 p-0">
        <ul class="left-panel">
          <li class="milestone">
            <div>
              <span class="count">2</span>
              <span class="number">1</span>
            </div>Milestone 1
          </li>
          <li class="divider"></li>
          <li class="fc-btn-month" @click="viewMonth">
            <i class="fas fa-calendar"></i>
            <span>Month</span>
          </li>
          <li class="fc-btn-week active" @click="viewWeek">
            <i class="fas fa-calendar"></i>
            <span>Week</span>
          </li>
          <li class="fc-btn-day" @click="viewDay">
            <i class="fas fa-calendar"></i>
            <span>Day</span>
          </li>
        </ul>
      </div>
      <div class="col">
        <ul class="center-panel">
          <li class="arrow-previous">
            <i class="fas fa-arrow-left"></i>
          </li>
          <li class="current-year">{{currentYear}}</li>
          <li
            class="month"
            :class="{ active: (index == currentMonth)}"
            v-for="(month, index) in months"
            :key="index"
          >
            <span>{{ month }}</span>
          </li>
          <li class="arrow-next">
            <i class="fas fa-arrow-right"></i>
          </li>
        </ul>
      </div>
    </div>
    <div class="row">
      <div class="col" v-if="currentView === 'dayGridMonth'">graph here</div>
      <div
        :class="{ 'col-6': currentView === 'dayGridMonth', col: currentView !== 'dayGridMonth' }"
      >
        <div class="controls">
          <FullCalendar
            class="app-calendar"
            ref="fullCalendar"
            defaultView="dayGridMonth"
            themeSystem="bootstrap"
            :header="{
              left: '',
              center: '',
              right: ''
            }"
            :plugins="calendarPlugins"
            :weekends="calendarWeekends"
            :events="calendarEvents"
            @dateClick="handleDateClick"
          />

          <div class="next" @click="nextMonth">
            <i class="fas fa-arrow-up"></i>
          </div>
          <div class="prev" @click="previousMonth">
            <i class="fas fa-arrow-down"></i>
          </div>
        </div>
      </div>
      <div class="col" v-if="currentView === 'dayGridMonth'">
        <FullCalendar
          class="app-calendar-day"
          ref="dayCalendar"
          defaultView="timeGridDay"
          themeSystem="bootstrap"
          :header="{
            left: '',
            center: '',
            right: ''
            }"
          :plugins="calendarPlugins"
          :weekends="calendarWeekends"
          :events="calendarEvents"
        />
      </div>
    </div>
    <!-- <div class="demo-app-top">
      <button @click="toggleWeekends">toggle weekends</button>
      <button @click="gotoPast">go to a date in the past</button>
      (also, click a date/time to add an event)
    </div>-->
  </div>
</template>

<script>
import FullCalendar from "@fullcalendar/vue";
import dayGridPlugin from "@fullcalendar/daygrid";
import timeGridPlugin from "@fullcalendar/timegrid";
import interactionPlugin from "@fullcalendar/interaction";
import bootstrapPlugin from "@fullcalendar/bootstrap";
export default {
  components: {
    FullCalendar
  },
  data: function() {
    return {
      months: [
        "JAN",
        "FEB",
        "MAR",
        "APR",
        "MAY",
        "JUN",
        "JUL",
        "AUG",
        "SEP",
        "OCT",
        "NOV",
        "DEC"
      ],
      calendar: null,
      currentView: "dayGridMonth",
      currentMonth: 0,
      currentYear: 0,
      calendarPlugins: [
        bootstrapPlugin,
        dayGridPlugin,
        timeGridPlugin,
        interactionPlugin
      ],
      calendarWeekends: true,
      calendarEvents: [
        // initial event data
        { title: "Event Now", start: new Date() }
      ]
    };
  },
  mounted() {
    var calendar = this.$refs.fullCalendar.getApi();
    var currentDate = calendar.getDate();
    this.currentMonth = currentDate.getMonth();
    this.currentYear = currentDate.getFullYear();
    calendar.setOption("height", 600);
  },
  methods: {
    nextMonth() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.next();
      var currentDate = calendar.getDate();
      this.currentMonth = currentDate.getMonth();
      this.currentYear = currentDate.getFullYear();
    },
    previousMonth() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.prev();
      var currentDate = calendar.getDate();
      this.currentMonth = currentDate.getMonth();
      this.currentYear = currentDate.getFullYear();
    },
    viewMonth() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.changeView("dayGridMonth");
      this.currentView = "dayGridMonth";
    },
    viewWeek() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.changeView("timeGridWeek");
      this.currentView = "timeGridWeek";
    },
    viewDay() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.changeView("timeGridDay");
      this.currentView = "timeGridDay";
    },
    handleDateClick(arg) {
      var calendar = this.$refs.dayCalendar.getApi();
      calendar.gotoDate(arg.date);
    }
  }
};
</script>

<style lang='scss'>
@import "~@fullcalendar/core/main.css";
@import "~@fullcalendar/daygrid/main.css";
@import "~@fullcalendar/timegrid/main.css";

@import "~bootstrap/dist/css/bootstrap.css";

.wrapper {
  .top-panel {
    height: 65px;
    border: 1px solid #ddd;
    background-color: #f7f7f7;
    margin-bottom: 10px;

    .left-panel {
      list-style: none;
      margin: 0;
      padding: 8px;

      li {
        display: inline-block;
        text-align: center;
        .fas {
          width: 100%;
          clear: both;
        }

        &.divider {
          border-right: 1px solid #ddd;
          height: 42px;
        }

        &.milestone > div {
          position: relative;
          text-align: center;
          border: 3px solid #c4c4c4;
          border-radius: 17px;
          width: 35px;
          height: 35px;
          padding: 0;
          float: left;
          line-height: 13px;
          margin-right: 10px;
          .count {
            position: absolute;
            top: 0;
            right: 0;
          }
        }

        &[class*="fc-btn"] {
          &:hover,
          &:active {
            cursor: pointer;
          }
          &.active {
            color: #1be6ff;
          }
        }
      }
    }

    .center-panel {
      list-style: none;
      margin: 0;
      padding: 0;

      li {
        display: inline-block;
        text-align: center;

        &.month {
          padding: 20px 22px;

          &.active {
            color: #1be6ff;
          }
        }
      }
    }
  }

  .controls {
    position: relative;
    z-index: 9999;
    .next {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      margin-left: auto;
      margin-right: auto;
      width: 16px;
    }
    .prev {
      position: absolute;
      bottom: -30px;
      left: 0;
      right: 0;
      margin-left: auto;
      margin-right: auto;
      width: 16px;
    }
  }
}

.app-calendar {
  margin: 0 auto;

  .fc-view-container {
    table > thead:after {
      content: "-";
      display: block;
      line-height: 1em;
      color: transparent;
    }
  }
}
</style>