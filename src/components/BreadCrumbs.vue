<template>
  <div class="q-pa-md q-gutter-sm">
    <q-breadcrumbs active-color="white">
      <!-- <q-breadcrumbs-el
        active-class="active-link_color__orange"
        :label="currentChildUrl"
        icon="widgets"
        :to="'/' + currentChildUrl"
        v-if="currentChildUrl"
      />
      <q-breadcrumbs-el
        active-class="active-link_color__orange"
        label="Home"
        icon="home"
        to="/"
        tag="a"
        v-else
      /> -->
      <q-breadcrumbs-el
        active-class="active-link_color__orange"
        :label="mainPathFormated"
        icon="home"
        :to="'/' + mainPath"
        tag="a"
        v-if="mainPathFormated"
      />
      <q-breadcrumbs-el
        active-class="active-link_color__orange"
        label="Home"
        icon="home"
        to="/"
        tag="a"
        v-else
      />
      <q-breadcrumbs-el
        active-class="active-link_color__orange"
        :label="childrenPath"
        :to="childrenPath"
        tag="a"
        v-if="childrenPath"
      />
      <template> </template>
      <template> </template>
    </q-breadcrumbs>
  </div>
</template>
<script setup lang="ts">
import { computed } from 'vue';
import { useRouter } from 'vue-router';

type RoutesLinksType = {
  [key: string]: string;
};

const router = useRouter();
const routesLinks: RoutesLinksType = {
  tickets: 'Tickets',
  ticket: 'Ticket',
  login: 'Login',
  profile: 'Profile',
};

const filteredStringUrl = computed(() => {
  const filteredString = router.currentRoute.value.path
    .replace('/', ' ')
    .split('/');
  return filteredString;
});

const mainPath = computed(() => {
  return filteredStringUrl.value[0].trim();
});

const mainPathFormated = computed(() => {
  return routesLinks[filteredStringUrl.value[0].trim()];
});
const childrenPath = computed(() => {
  return filteredStringUrl.value[1];
});
</script>
<style lang="scss">
.active-link {
  &_color {
    &__orange {
      color: #fa9435;
    }
  }
}
</style>
