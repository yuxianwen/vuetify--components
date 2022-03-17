<template>
  <span class="countdown">
    {{ timer }}
  </span>
</template>

<script>
import countdown from "countdown";
import moment from "moment";
export default {
  props: {
    date: { type: String, default: "" },
    end: {
      type: [Object, String, Date, Number],
      default: () => {
        return moment().add(30, "minutes");
      }
    },
    second: { type: Boolean, default: true },
    minute: { type: Boolean, default: true },
    hour: { type: Boolean, default: true },
    day: { type: Boolean, default: true }
  },
  data() {
    return {
      now: Math.trunc(new Date().getTime() / 1000),
      timerId: null,
      timer: ""
    };
  },

  mounted() {
    // 初始化倒计时
    // console.log(new Date(this.end.replace(/-/g, "/")), this.end);
    this.timerId = countdown(
      ["string", "number"].includes(typeof this.end)
        ? new Date(this.end.replace(/-/g, "/"))
        : this.end,
      ts => {
        this.timer = ` ${this.day ? ts.days + "天" : ""}${
          this.hour ? ts.hours + "时" : ""
        }${ts.minutes}分${ts.seconds}秒`;

        if (ts.value >= 0) {
          this.$emit("end");
        }
      },
      countdown.DAYS | countdown.HOURS | countdown.MINUTES | countdown.SECONDS
    );
  },
  destroyed() {
    window.clearInterval(this.timerId);
  }
};
</script>
