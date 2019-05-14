<template>
  <div class="container-fluid wrapper">
    <div class="row top-panel">
      <div class="col-3 p-0">
        <ul class="left-panel">
          <li class="milestone">
            <span class="count">2</span>
            <span class="number">1</span>
            <span class="label">Milestone 1</span>
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
          <li class="month" :class="getCurrentMonth" v-for="(month, index) in months" :key="index">
            <span>{{ month }}</span>
          </li>
          <li class="arrow-next">
            <i class="fas fa-arrow-right"></i>
          </li>
        </ul>
      </div>
    </div>
    <div class="row">
      <div class="col">graph here</div>
      <div class="col-6">
        <FullCalendar
          class="demo-app-calendar"
          ref="fullCalendar"
          defaultView="dayGridMonth"
          themeSystem="bootstrap"
          :header="{
        left: '',
        center: 'title',
        right: 'dayGridMonth,timeGridWeek,timeGridDay,listWeek'
      }"
          :customButtons="{
          dayGridMonth: {
              text: 'sample month',

          }
      }"
          :plugins="calendarPlugins"
          :weekends="calendarWeekends"
          :events="calendarEvents"
          @dateClick="handleDateClick"
        />
      </div>
      <div class="col">currently selected day here</div>
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
    FullCalendar // make the <FullCalendar> tag available
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
      currentMonth: 0,
      calendarPlugins: [
        // plugins must be defined in the JS
        bootstrapPlugin,
        dayGridPlugin,
        timeGridPlugin,
        interactionPlugin // needed for dateClick
      ],
      calendarWeekends: true,
      calendarEvents: [
        // initial event data
        { title: "Event Now", start: new Date() }
      ]
    };
  },
  methods: {
    toggleWeekends() {
      this.calendarWeekends = !this.calendarWeekends; // update a property
    },
    nextMonth() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.next();
    },
    previousMonth() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.prev();
    },
    viewMonth() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.changeView("dayGridMonth");
    },
    viewWeek() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.changeView("timeGridWeek");
    },
    viewDay() {
      var calendar = this.$refs.fullCalendar.getApi();
      calendar.changeView("timeGridDay");
    },
    getCurrentMonth() {
      var currentDate = this.calendar.getDate();
      this.currentMonth = currentDate.getMonth();
      return "test";
    },
    gotoPast() {
      let calendarApi = this.calendar.getApi(); // from the ref="..."
      calendarApi.gotoDate("2000-01-01"); // call a method on the Calendar object
    },
    handleDateClick(arg) {
      if (confirm("Would you like to add an event to " + arg.dateStr + " ?")) {
        this.calendarEvents.push({
          // add new event data
          title: "New Event",
          start: arg.date,
          allDay: arg.allDay
        });
      }
    }
  }
};
</script>

<style lang='scss'>
// you must include each plugins' css
// paths prefixed with ~ signify node_modules
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
      padding: 0;

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

        &.milestone {
          position: relative;
          text-align: center;
          .count {
            position: absolute;
            top: 0;
            right: 0;
          }
          .number {
            display: block;
            padding: 13px;
            border: 2px solid;
            border-radius: 28px;
            width: 54px;
          }
        }

        &[class*="fc-btn"] {
          &:hover,
          &:active {
            cursor: pointer;
          }
          &.active {
            color: red;
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
        }
      }
    }
  }
}

.demo-app {
  font-family: Arial, Helvetica Neue, Helvetica, sans-serif;
  font-size: 14px;
}
.demo-app-top {
  margin: 0 0 3em;
}
.demo-app-calendar {
  margin: 0 auto;
  //   max-width: 900px;

  //   .fc-toolbar.fc-header-toolbar {
  //     height: 65px;
  //     border-bottom: 1px solid #ddd;
  //     background-color: #f7f7f7;
  //   }
}
</style>