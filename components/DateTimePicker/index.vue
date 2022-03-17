<template>
  <div class="d-inline-block">
    <v-menu
      v-model="menu"
      :close-on-content-click="false"
      :close-on-click="false"
      :nudge-bottom="40"
      transition="scale-transition"
      min-width="100"
      bottom
    >
      <template v-slot:activator="{ on }">
        <v-text-field
          :value.sync="dateTime"
          append-icon="mdi-calendar"
          @click:clear="clearDateTime"
          :disabled="readonly"
          v-bind="$attrs"
          v-on="on"
        ></v-text-field>
      </template>
      <v-card class="text-right">
        <v-divider />

        <div class="d-flex">
          <v-date-picker
            no-title
            v-model="date"
            @change="dateChange"
            elevation="0"
          ></v-date-picker>
          <v-divider class="ml-1 mr-1" vertical />

          <v-time-picker
            v-model="time"
            @change="timeChange"
            elevation="0"
            no-title
          ></v-time-picker>
        </div>
        <v-divider />
        <v-btn outlined color="primary" small class=" ma-2" @click="setTime"
          >确定</v-btn
        >
      </v-card>
    </v-menu>
    <template v-if="isRange">
      <div class=" ma-2">至</div>
      <v-menu
        v-model="menu2"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        min-width="100"
        offset-y
      >
        <template v-slot:activator="{ on }">
          <v-text-field
            :value.sync="dateTime"
            append-icon="mdi-calendar"
            @click:clear="clearDateTime"
            dense
            outlined
            height="40"
            v-bind="$attrs"
            v-on="on"
          ></v-text-field>
        </template>
        <v-card class="text-right">
          <div class="d-flex">
            <v-date-picker v-model="date" @change="dateChange"></v-date-picker>
            <v-time-picker
              v-model="time"
              @change="timeChange"
              class="ml-3"
            ></v-time-picker>
          </div>
          <v-btn
            outlined
            color="primary"
            small
            class=" ma-2"
            @click="menu2 = false"
            >确定</v-btn
          >
        </v-card>
      </v-menu>
    </template>
  </div>
</template>

<script>
export default {
  props: {
    value: {},
    type: { type: String },
    readonly: { type: Boolean, default: false }
  },
  date() {
    return {
      menu: false,
      menu2: false,
      date: "",
      time: "",
      dateTime: ""
    };
  },
  computed: {
    isRange() {
      return this.type === "datetimerange";
    }
  },
  methods: {
    dateChange(val) {
      this.date = val;
    },
    timeChange(val) {
      this.time = val;
    },
    setTime() {
      const time = `${this.date} ${this.time}`;
      this.menu = false;
      this.$emit("input", time);
    },
    clearDateTime() {
      this.date = "";
      this.time = "";
    }
  },

  created() {
    if (this.value) {
      const [date, time] = this.value && this.value.split(" ");
      this.date = date;
      this.time = time;
      this.dateTime = `${this.date} ${this.time}`;
    }

    if (this.readonly) {
      this.menu = false;
    }
  }
};
</script>

<style></style>
