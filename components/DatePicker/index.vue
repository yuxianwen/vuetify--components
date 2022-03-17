<template>
  <v-menu
    v-model="menu"
    :close-on-content-click="false"
    :nudge-left="0"
    :nudge-top="25"
    :close-on-click="!isRange"
    transition="scale-transition"
    offset-y
    min-width="290px"
  >
    <template v-slot:activator="{ on }">
      <v-text-field
        v-model="date"
        :class="className"
        prepend-inner-icon="mdi-calendar"
        readonly
        v-bind="$attrs"
        v-on="on"
      ></v-text-field>
    </template>
    <v-date-picker
      v-model="date"
      no-title
      scrollable
      v-bind="$attrs.picker ? $attrs.picker : ''"
      @input="pickerChange"
    ></v-date-picker>
  </v-menu>
</template>

<script>
import { parseTime } from "@/utils";

export default {
  data() {
    return {
      menu: false,
      date: ""
    };
  },

  created() {
    this.date = this.default
      ? this.value || parseTime(new Date(), this.format())
      : "";
  },

  props: {
    value: {
      type: [String, Array]
    },
    className: { type: String },
    default: { type: Boolean, default: true }
  },

  watch: {
    value(val) {
      this.date = val;
    },
    date(val) {
      this.$emit("input", val);
      this.$emit("change", val);
    }
  },
  computed: {
    isRange() {
      return !!this.$attrs.range;
    }
  },
  methods: {
    pickerChange(val) {
      console.log(val);
      if (this.isRange) {
        val = val.filter(n => n);
        val.length === 2 && (this.menu = false);
      } else {
        this.menu = false;
      }
    },
    format() {
      const type = this.$attrs.picker ? this.$attrs.picker.type : "";
      if (type === "month") {
        return "{y}-{m}";
      } else if (type === "year") {
        return "{y}";
      } else {
        return "{y}-{m}-{d}";
      }
    }
  }
};
</script>

<style></style>
