<template>
  <v-avatar width="40" height="40" min-width="40">
    <v-img v-if="src" :src="src" @error="imgError" />
    <v-icon v-else size="40" color="gray1">mdi-account-circle</v-icon>
  </v-avatar>
</template>

<script>
import { defineComponent } from "@vue/composition-api";

export default defineComponent({
  props: {
    src: String
  },
  setup() {
    const imgError = () => {
      this.src = false;
    };
    return {
      imgError
    };
  }
});
</script>
