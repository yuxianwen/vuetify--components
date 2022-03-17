<template>
  <v-card width="132" elevation="0" color="transparent">
    <v-text-field dense outlined>
      <template v-slot:append-outer>
        <v-btn icon class="mt-n1">
          <v-icon>
            mdi-plus
          </v-icon>
        </v-btn>
      </template>
      <template v-slot:prepend>
        <v-btn icon class="mt-n1">
          <v-icon>
            mdi-minus
          </v-icon>
        </v-btn>
      </template>
    </v-text-field>
  </v-card>
</template>
