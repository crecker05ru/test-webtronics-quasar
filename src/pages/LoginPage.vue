<template>
  <q-page class="row items-center justify-evenly">
    <div class="q-pa-md" style="min-width: 400px">
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input
          filled
          v-model="name"
          label="Your name"
          hint="Name"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-input
          filled
          v-model="password"
          :type="isPassword ? 'password' : 'text'"
          label="Password"
          lazy-rules
          :rules="[
            (val) => (val && val.length > 2) || 'Please type more 2 letters',
          ]"
        >
          <template v-slot:append>
            <q-icon
              :name="isPassword ? 'visibility_off' : 'visibility'"
              class="cursor-pointer"
              @click="isPassword = !isPassword"
            ></q-icon>
          </template>
        </q-input>

        <!-- <q-toggle v-model="accept" label="I accept the license and terms" /> -->

        <div>
          <q-btn label="Submit" type="submit" color="primary" />
          <q-btn
            label="Reset"
            type="reset"
            color="primary"
            flat
            class="q-ml-sm"
          />
        </div>
      </q-form>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useQuasar } from 'quasar';
import { useRouter } from 'vue-router';
const $q = useQuasar();
const name = ref<string>('');
const password = ref<string>('');
const isPassword = ref<boolean>(true);
const router = useRouter();

const onSubmit = () => {
  const postData = {
    name: name.value,
    password: password.value,
  };
  $q.notify({
    color: 'green-4',
    textColor: 'white',
    icon: 'cloud_done',
    message: 'Success',
  });
  localStorage.setItem('isLogged', 'true');
  localStorage.setItem('userData', postData.name);
  setTimeout(() => {
    router.push('/');
  }, 1000);
};

const onReset = () => {
  name.value = '';
  password.value = '';
  $q.notify({
    color: 'green-4',
    textColor: 'white',
    icon: 'cloud_done',
    message: 'Reset done',
  });
};
</script>
