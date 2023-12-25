<template>
  <div class="profile">
    <q-page class="row items-center justify-evenly">
      <div class="q-pa-md">
        <div class="q-gutter-md">
          <q-field label="Name" stack-label>
            <template v-slot:control>
              <div class="self-center full-width no-outline" tabindex="0">
                {{ userData.name }}
              </div>
            </template>
          </q-field>
          <q-field label="Surname" stack-label>
            <template v-slot:control>
              <div class="self-center full-width no-outline" tabindex="0">
                {{ userData.surname }}
              </div>
            </template>
          </q-field>
          <q-input
            v-model="userData.birthDate"
            type="date"
            label="Birth date"
          />
          <q-select
            color="purple-12"
            v-model="choosenCity"
            :options="cities"
            label="City"
          />
        </div>
      </div>
    </q-page>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
const userData = {
  information: 'Information',
  name: 'Anvar',
  surname: 'Abduragimov',
  birthDate: '1991-02-12',
  city: 'St.Peterburg',
};

const cities = ref(['Luxemburg', 'Marcel', 'London', 'Moscow', 'St.Peterburg']);
const choosenCity = ref(userData.city);
</script>
