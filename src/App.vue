<template>
  <v-app>
    <v-main class="ma-5">
      <h1 class="text-center">Weathery Space App</h1>

      <!-- Choose mode -->
      <v-tabs class="my-5">
        <v-tab>Forecast</v-tab>
        <v-tab>History</v-tab>

        <!-- Forecasting tab -->
        <v-tab-item>
          <!-- Query fields -->
          <div class="d-flex align-center mt-5">
            <v-select
              v-model="location"
              :items="locations"
              outlined
              dense
              hide-details
              label="Choose a location"
            ></v-select>
            <v-btn
              color="success"
              depressed
              class="ml-3"
              :loading="forecastLoading"
              @click="forecast()"
            >Predict!</v-btn>
          </div>

          <!-- Display weather stats -->
          <v-row
            v-if="items.length > 0"
            class="my-5"
          >
            <v-col
              v-for="(item, index) in items" :key="index"
              md="3"
            >
              <v-card class="mx-1">
                <v-list-item two-line>
                  <v-list-item-content>
                    <v-list-item-title class="text-h5">
                      {{ item.conditions }}
                    </v-list-item-title>
                    <v-list-item-subtitle>{{ item.datetime }}, {{ item.description }}</v-list-item-subtitle>
                  </v-list-item-content>
                </v-list-item>

                <v-card-text>
                  <v-row align="center">
                    <v-col
                      class="text-h4"
                      cols="8"
                    >
                      {{ item.temp }}&deg;F
                    </v-col>
                    <v-col cols="4">
                      <v-img
                        :src="`https://cdn.vuetifyjs.com/images/cards/${'sun'}.png`"
                        width="92"
                      ></v-img>
                    </v-col>
                  </v-row>
                </v-card-text>

                <v-divider></v-divider>

                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>mdi-send</v-icon>
                  </v-list-item-icon>
                  <v-list-item-subtitle>{{ item.windspeed }} mph</v-list-item-subtitle>
                </v-list-item>

                <v-divider></v-divider>

                <v-list-item>
                  <v-list-item-icon>
                    <v-icon>mdi-cloud</v-icon>
                  </v-list-item-icon>
                  <v-list-item-subtitle>{{ item.cloudcover }}% cloud cover</v-list-item-subtitle>
                </v-list-item>
              </v-card>
            </v-col>
          </v-row>
          <p
            v-else
            class="text-center my-8"
          >
            No data yet. Select a location to start!
          </p>
        </v-tab-item>

        <!-- History tab -->
        <v-tab-item>

        </v-tab-item>
      </v-tabs>


    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data: () => ({
    location: '',
    items: [],
    locations: ['Brunei', 'Australia', 'India', 'Japan', 'China', 'Malaysia', 'Indonesia'],
    forecastLoading: false,
  }),
  methods: {
    async forecast() {
      this.forecastLoading = true
      await axios.get('http://localhost:3000/forecast', { params: { location: this.location } }).then(res => {
        const { data } = res
        this.items = data.data.days
        this.forecastLoading = false
      }).catch(() => {})
    }
  }
};
</script>

<style lang="scss">
  .row {
    flex-wrap: nowrap !important;
    overflow: auto !important;
  }
</style>
