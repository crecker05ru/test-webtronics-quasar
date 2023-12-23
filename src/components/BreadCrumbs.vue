<template>
  <div class="q-pa-md q-gutter-sm">
    <q-breadcrumbs active-color="white">
      <q-breadcrumbs-el
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
      />
    </q-breadcrumbs>
  </div>
</template>
<script setup lang="ts">
import { ref, computed, watch, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';

type RoutesLinksType = {
  [key: string]: string;
};

const router = useRouter();
const route = useRoute();
const routesLinks: RoutesLinksType = {
  tickets: 'Tickets',
  login: 'Login',
  profile: 'Profile',
};

// const currentPath = ref<string>(window.location.pathname);
const currentHashPath = ref<string>(window.location.hash);
// const currentHashPath = computed<string>(() => {
//   console.log('window.location.hash', window.location.hash);
//   return window.location.hash;
// });

const currentChildUrl = computed(() => {
  const filteredString = router.currentRoute.value.path.replace(/\//g, '');
  return routesLinks[filteredString];
});
watch(currentHashPath, (newVal) => {
  // currentHashPath.value = window.location.hash;
  console.log('currentHashPath.value', currentHashPath.value);
  console.log('newVa', newVal);
  console.log('currentChildUrl.value', currentChildUrl.value);
});
onMounted(() => {
  console.log(route.hash);
  console.log(router.currentRoute.value.path);
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
