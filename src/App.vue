<template>
  <v-app>
    <v-main class="ma-5">
      <h1 class="text-center">Weathery Space App</h1>
      <v-snackbar
        v-model="errorSnackbar"
        color="error"
        :top="true"
        :right="true"
        outlined
      >
        {{ errorMessage }}
        <template v-slot:action="{ attrs }">
          <v-btn
            color="red"
            text
            v-bind="attrs"
            @click="errorSnackbar = false"
          >
            Close
          </v-btn>
        </template>
      </v-snackbar>

      <!-- Choose mode -->
      <v-tabs class="my-5">
        <v-tab>Forecast</v-tab>
        <v-tab>History</v-tab>

        <!-- Forecasting tab -->
        <v-tab-item>
          <p class="mt-3">This mode predicts the weather pattern for the next 7 days for a given location.</p>
          <!-- Query fields -->
          <div
            class="mt-3"
            :class="{ 'd-flex align-center': $vuetify.breakpoint.smAndUp }"
          >
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
              :block="$vuetify.breakpoint.xsOnly"
              class="ml-sm-3"
              :class="{ 'mt-3': $vuetify.breakpoint.xsOnly }"
              :loading="forecastLoading"
              @click="forecast()"
            >Predict!</v-btn>
          </div>
        </v-tab-item>

        <!-- History tab -->
        <v-tab-item>
          <p class="mt-3">This mode lets you search trough weather history between 2 dates.</p>
          <!-- Query fields -->
          <div
            class="mt-3"
            :class="{ 'd-flex align-center': $vuetify.breakpoint.smAndUp }"
          >
            <v-select
              v-model="location"
              :items="locations"
              outlined
              dense
              hide-details
              label="Choose a location"
            ></v-select>
            <v-menu>
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="startDate"
                  label="Choose Start Date"
                  outlined
                  dense
                  hide-details
                  class="ml-sm-3"
                  :class="{ 'mt-3': $vuetify.breakpoint.xsOnly }"
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="startDate"
                no-title
              ></v-date-picker>
            </v-menu>
            <v-menu>
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="endDate"
                  label="Choose End Date"
                  outlined
                  dense
                  hide-details
                  class="ml-sm-3"
                  :class="{ 'mt-3': $vuetify.breakpoint.xsOnly }"
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="endDate"
                no-title
              ></v-date-picker>
            </v-menu>
            <v-btn
              color="success"
              depressed
              :block="$vuetify.breakpoint.xsOnly"
              class="ml-sm-3"
              :class="{ 'mt-3': $vuetify.breakpoint.xsOnly }"
              :loading="historyLoading"
              @click="history()"
            >Search!</v-btn>
          </div>
        </v-tab-item>
      </v-tabs>

      <!-- Display weather stats -->
      <v-row
        v-if="items.length > 0"
        class="my-5"
        :class="{ 'side-scroll': $vuetify.breakpoint.smAndUp }"
      >
        <v-col
          v-for="(item, index) in items" :key="index"
          md="3"
        >
          <weather-card :weatherStats="item"></weather-card>
        </v-col>
      </v-row>
      <p
        v-else
        class="text-center my-8"
      >
        No data yet. Enter the fields to start!
      </p>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'
import { WeatherCard } from '@/components'

export default {
  name: 'App',
  components: { WeatherCard },
  data: () => ({
    location: null,
    startDate: null,
    endDate: null,
    items: [],
    locations: ['Brunei', 'Australia', 'India', 'Japan', 'China', 'Malaysia', 'Indonesia'],
    forecastLoading: false,
    historyLoading: false,
    errorMessage: null,
    errorSnackbar: false
  }),
  methods: {
    async forecast() {
      this.forecastLoading = true
      await axios.get(`${process.env.VUE_APP_API_URL}/forecast`, {
        params: {
          location: this.location
        }
      }).then(res => {
        const { data } = res
        this.items = data.data.days
        this.forecastLoading = false
      }).catch(err => {
        this.errorMessage = err.response.data.message || 'Something went wrong. Please refresh'
        this.forecastLoading = false
      })
    },
    async history() {
      this.historyLoading = true
      await axios.get(`${process.env.VUE_APP_API_URL}/history`, {
        params: {
          location: this.location,
          start_date: this.startDate,
          end_date: this.endDate
        }
      }).then(res => {
        const { data } = res
        this.items = data.data.days
        this.historyLoading = false
      }).catch(err => {
        this.errorMessage = err.response.data.message || 'Something went wrong. Please refresh'
        this.historyLoading = false
      })
    }
  }
};
</script>

<style lang="scss">
  .side-scroll {
    flex-wrap: nowrap !important;
    overflow: auto !important;
  }
</style>
