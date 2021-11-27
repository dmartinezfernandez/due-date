<template>
  <div>
    <h1>{{ title }}</h1>
    <p>
      When was the first day of your last period?
      <input
        v-model="firstDay"
        placeholder="yyyy-MM-dd"
        pattern="[0-9]{4}-[0-9]{2}-[0-9]{2}"
        style="width: 76px;"
      />
      <span class="darker" v-show="showWarning">&emsp;Invalid date.</span>
    </p>
    <p>
      Your results:
      <br />
      You are {{weeks}} pregnant.
      <br />
      Your due date is {{dueDate}}.
    </p>
    <details>
      <summary>Rationale</summary>
      <br />
      The average length of human gestation is 280 days, or 40 weeks, from the first day of the woman’s last menstrual period.<br>
      You can use this calculator by adding the first day to the end of the URL in the format of '?firstDay=yyyy-MM-dd'.
    </details>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "DueDate",
  props: {
    title: String
  },
  computed: {
    showWarning() {
      return this.firstDay && isNaN(Date.parse(this.firstDay));
    },
    dueDate() {
      if (!this.showWarning) {
        let date = new Date(this.firstDay);
        date.setDate(date.getDate() + 280);
        return date.toDateString();
      } else return NaN;
    },
    weeks() {
      if (!this.showWarning) {
        let date0 = new Date(this.firstDay),
          date1 = new Date();
        let days = Math.floor(
          (Date.UTC(date1.getFullYear(), date1.getMonth(), date1.getDate()) -
            Date.UTC(date0.getFullYear(), date0.getMonth(), date0.getDate())) /
            (1000 * 60 * 60 * 24)
        );
        let daysPart = days % 7;
        let weeksPart = (days - daysPart) / 7;
        return (
          weeksPart +
          " week" +
          (weeksPart == 1 ? "" : "s") +
          (daysPart > 0
            ? " and " + daysPart + " day" + (daysPart == 1 ? "" : "s")
            : "")
        );
      } else return NaN;
    }
  },
  setup() {
    console.log("setup()");
    let queryString = new URLSearchParams(window.location.search);
    let qsFirstDay = queryString.get("firstDay");
    let warning = undefined,
      firstDateValue = undefined;
    if (Date.parse(qsFirstDay) != NaN) {
      firstDateValue = new Date(qsFirstDay);
      console.log(firstDateValue);
    }
    console.log(qsFirstDay);
    const firstDayWarning = ref(warning);
    const firstDay = ref(qsFirstDay ? qsFirstDay : "");

    // expose to template (https://composition-api.vuejs.org/api.html#setup)
    return {
      firstDay,
      firstDayWarning
    };
  }
};
</script>

<style scoped>
input {
  border-width: 0px 0px 1px 0px;
}
</style>
