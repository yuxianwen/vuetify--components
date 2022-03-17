<template>
  <v-menu
    ref="menu"
    v-model="menu"
    :close-on-content-click="false"
    :nudge-left="0"
    :nudge-top="24"
    :return-value.sync="time"
    transition="scale-transition"
    offset-y
    max-width="290px"
    min-width="290px"
  >
    <template v-slot:activator="{ on }">
      <v-text-field
        v-model="time"
        append-icon="mdi-clock-outline"
        readonly
        v-bind="$attrs"
        v-on="on"
      ></v-text-field>
    </template>
    <v-time-picker
      v-if="menu"
      v-model="time"
      full-width
      no-title
      v-bind="$attrs.picker ? $attrs.picker : ''"
      @click:minute="$refs.menu.save(time)"
    ></v-time-picker>
  </v-menu>
</template>

<script>
import { parseTime } from "@/utils";

export default {
  props: { value: { type: String }, hint: { type: String }, now: Boolean },
  data() {
    return {
      menu: false,
      time: ""
    };
  },
  watch: {
    value(val) {
      this.time = val;
    },
    time(val) {
      this.$emit("input", val);
    }
  },

  created() {
    this.time = this.value || this.now ? parseTime(new Date(), "{h}:{m}") : "";
  }
};
</script>
