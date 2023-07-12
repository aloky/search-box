<script setup>
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css'

import { ref, watch } from 'vue';

const element = document.getElementById('app');

const cities = element.cities;
const from = ref(null);
const to = ref(null);

const withoutEndDate = ref(false)

watch(withoutEndDate, () => {
    selectDate.value = null;
});

const selectDate = ref(null);

const search = () => {
  element.dispatchEvent(new CustomEvent('search', {
    detail: {
      from: from.value,
      to: to.value,
      selectedDate: selectDate.value
    },
  }));
}
</script>

<template>
  <div class="d-flex align-center h-100">
    <v-card flat rounded="lg" class="bg-white pa-4 mx-auto" width="1280">
      <v-row class="align-end">
        <v-col cols="12" sm="6" md="3">
          <label>
            <div class="mb-2">
              Откуда
            </div>
            <v-autocomplete
              v-model="from"
              hide-details="auto"
              prepend-inner-icon="mdi-map-marker"
              density="comfortable"
              variant="solo"
              :items="cities"
            />
          </label>
        </v-col>
        <v-col cols="12" sm="6" md="3">
          <label>
            <div class="mb-2">
              Куда
            </div>
            <v-autocomplete
              v-model="to"
              hide-details="auto"
              prepend-inner-icon="mdi-map-marker"
              density="comfortable"
              variant="solo"
              :items="cities"
            />
          </label>
        </v-col>
        <v-col cols="12" sm="8" md="4">
          <div class="mb-2">
            Даты
          </div>
          <VueDatePicker
            :key="withoutEndDate"
            v-model="selectDate"
            :range="!withoutEndDate"
            locale="ru"
            :min-date="new Date()"
            :month-change-on-scroll="false"
            :enable-time-picker="false"
          >
          <template #action-row="{ selectDate }">
            <div class="w-100 d-flex align-center justify-around-between">
              <v-checkbox
                hide-details v-model="withoutEndDate"
              >
                <template #label>
                  <div class="text-caption">
                    Без конечной даты
                  </div>
                </template>
              </v-checkbox>
              <v-btn color="grey-darken-4" @click="selectDate">Готово</v-btn>
            </div>
          </template>
      </VueDatePicker>
        </v-col>
        <v-col cols="12" sm="4" md="2">
          <v-btn class="text-none" block color="grey-darken-4" size="x-large"
            @click="search"
          >
            Найти
          </v-btn>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>
