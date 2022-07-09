<template>
  <v-card class="mx-1">
    <v-list-item two-line>
      <v-list-item-content>
        <v-list-item-title class="text-h5 mb-3">
          {{ weatherStats.datetime }}
        </v-list-item-title>
        <v-list-item-subtitle class="mb-2">{{ weatherStats.conditions }}</v-list-item-subtitle>
        <small>{{ weatherStats.description }}</small>
      </v-list-item-content>
    </v-list-item>

    <v-divider></v-divider>

    <v-card-text>
      <v-row align="center">
        <v-col
          class="text-h4"
          cols="8"
        >
          {{ weatherStats.temp }}&deg;F
        </v-col>
        <v-col cols="4">
          <v-img
            :src="require(`@/assets/${weatherStats.icon}.png`)"
            width="92"
          ></v-img>
        </v-col>
      </v-row>
    </v-card-text>

    <v-divider></v-divider>

    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-weather-windy</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>{{ weatherStats.windspeed }} mph</v-list-item-subtitle>
    </v-list-item>

    <v-divider></v-divider>

    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-cloud</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>{{ weatherStats.cloudcover }}% cloud cover</v-list-item-subtitle>
    </v-list-item>

    <v-divider></v-divider>

    <v-list-item>
      <v-list-item-icon>
        <v-icon>mdi-weather-rainy</v-icon>
      </v-list-item-icon>
      <v-list-item-subtitle>{{ weatherStats.precipprob }}% will rain</v-list-item-subtitle>
    </v-list-item>
  </v-card>
</template>

<script>
export default {
  props: {
    weatherStats: Object
  },
}
</script>