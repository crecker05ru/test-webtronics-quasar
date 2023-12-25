<template>
  <div class="tickets q-pa-md">
    <q-table
      class="tickets__table"
      title="Tickets"
      :rows="db"
      :columns="column"
      row-key="name"
      v-if="!isDetailsOpened"
      binary-state-sort
      :rows-per-page-options="[5, 10, 20]"
      rows-per-page-label="10"
      :filter="filterAuthor"
      :loading="isTableloading"
      @request="onRequest"
    >
      <template v-slot:top-right>
        <q-input
          borderless
          dense
          debounce="300"
          v-model="filterAuthor"
          placeholder="Search by author"
        >
          <template v-slot:append>
            <q-icon name="search" />
          </template>
        </q-input>
      </template>

      <template v-slot:header="props">
        <q-tr>
          <q-th v-for="col in props.cols" :key="col.name" :props="props">
            {{ col.label }}</q-th
          >
          <q-th> </q-th>
        </q-tr>
      </template>
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td v-for="col in props.cols" :key="col.name" :props="props">
            <template v-if="col.name !== 'url'">
              {{ col.value }}
            </template>
            <q-btn
              v-else
              color="purple"
              label="More"
              @click="showTicketDetails(col.value)"
            />
          </q-td>
        </q-tr>
        <!-- <q-tr v-show="props.expand" :props="props">
          <q-td colspan="100%">
            <div class="text-left">This is expand slot for row above: {{ props.row.name }}.</div>
          </q-td>
        </q-tr> -->
      </template>
    </q-table>
    <template v-else
      ><TicketDescription
        v-if="currentTicketDetails"
        :data="currentTicketDetails"
        @close-ticket="closeTicketDetails"
    /></template>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import TicketDescription from 'src/components/TicketDescription.vue';

type AlignType = 'left' | 'right' | 'center';
interface Book {
  id: number;
  title: string;
  author: string;
  genre: string;
  description: string;
  isbn: string;
  image: string;
  published: string | Date;
  publisher: string;
}

export interface Ticket {
  author: string;
  id: string;
  title: string;
  description: string;
  createdAt: string | Date;
  url: string;
}

const db = ref<Ticket[]>();
const isDetailsOpened = ref(false);
const filterAuthor = ref('');
const currentTicketDetails = ref<Ticket | undefined>();
const isTableloading = ref(false);
const showTicketDetails = (id: string) => {
  isDetailsOpened.value = true;
  currentTicketDetails.value = db.value?.find((item) => item.id === id);
};

const closeTicketDetails = () => {
  isDetailsOpened.value = false;
  currentTicketDetails.value = undefined;
};

const onRequest = () => {
  isTableloading.value = true;

  setTimeout(() => {
    const filteredData = db.value?.filter((item) => {
      item.author.includes(filterAuthor.value);
    });

    if (filteredData && filteredData?.length) {
      db.value?.splice(0, db.value?.length, ...filteredData);
    }

    isTableloading.value = false;
  }, 700);
};

const column = [
  {
    name: 'author',
    required: true,
    label: 'Author',
    field: 'author',
    sortable: true,
    align: 'left' as AlignType,
  },
  {
    name: 'id',
    required: true,
    label: 'Ticket id',
    field: 'id',
    sortable: true,
    align: 'left' as AlignType,
  },
  {
    name: 'title',
    required: true,
    label: 'Title',
    field: 'title',
    sortable: true,
    align: 'left' as AlignType,
  },
  {
    name: 'description',
    required: true,
    label: 'Description',
    field: (row: Ticket) => row?.description.slice(0, 20),
    sortable: false,
    align: 'left' as AlignType,
  },
  {
    name: 'createdAt',
    required: true,
    label: 'Created at',
    field: 'createdAt',
    sortable: true,
    align: 'left' as AlignType,
  },
  {
    name: 'url',
    required: false,
    label: '',
    field: (row: Ticket) => row?.id,
    sortable: false,
    align: 'left' as AlignType,
  },
];

onMounted(() => {
  (async () => {
    const resp = await fetch('/db/tickets.json');
    const body: Book[] = await resp.json();
    db.value = body.map((item) => {
      const formated: Ticket = {
        author: item.author,
        id: String(item.id),
        title: item.title,
        description: item.description,
        createdAt: item.published,
        url: `tickets/${item.id}`,
      };
      return formated;
    });
  })();
});
</script>
<style lang="scss">
.tickets {
  &__table {
    & td:first-child {
      background-color: #3f51b5;
      color: #fff;
    }
    & tr th {
      position: sticky;
      z-index: 2;
      background: #0090ce;
      color: #fff;
    }
  }
}
</style>
