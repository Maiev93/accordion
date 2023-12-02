<script setup>
import BaseDD from "./components/UI/BaseDD.vue";
import BaseAcc from "./components/UI/BaseAcc.vue";
import BaseLoader from "./components/UI/BaseLoader.vue";

import { ref, onMounted, watch } from "vue";

const opened = [0, 3, 6];
const perPage = 10;

const charData = ref(null);
const starshipsData = ref(null);
const currentPage = ref(1);
const pages = ref(0);
const isLoading = ref(false);

onMounted(() => {
  getDataChar();
  getDataStarships();
});

watch(currentPage, () => {
  getDataChar();
});

// functions
const setPage = (page) => {
  currentPage.value = page;
};
const getDataChar = async () => {
  isLoading.value = true;
  try {
    const response = await fetch(
      `https://swapi.dev/api/people?page=${currentPage.value}`
    );
    const data = await response.json();
    charData.value = data.results;
    pages.value = Math.ceil(data.count / perPage);
    console.log(pages.value);
  } catch (e) {
    throw new Error(e);
  } finally {
    isLoading.value = false;
  }
};
const getDataStarships = async () => {
  isLoading.value = true;
  try {
    const response = await fetch("https://swapi.dev/api/starships");
    const { results } = await response.json();
    starshipsData.value = results;
  } catch (e) {
    throw new Error(e);
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <BaseLoader v-if="isLoading" />

  <h1 class="title">Accordion component & SWAPI</h1>

  <section class="fetch" v-if="charData">
    <BaseAcc title="Base exapmle with async data">
      <BaseDD v-for="char in charData" :key="char.name" :titleDD="char.name">
        <p class="fetch__content">Birth year: {{ char.birth_year }}</p>
      </BaseDD>
    </BaseAcc>
    <ul class="fetch__pagination" v-if="pages">
      <li
        class="fetch__page"
        :class="{ fetch__page_active: page === currentPage }"
        v-for="page in pages"
        :key="page"
        @click="setPage(page)"
      >
        {{ page }}
      </li>
    </ul>
  </section>

  <section class="nested">
    <BaseAcc title="Exapmle with nested accordion">
      <BaseDD titleDD="First DD title">
        <BaseAcc title="Nested">
          <BaseDD titleDD="Second DD title">
            <p>
              Ornare lacinia nec et cras mattis mattis luctus amet, orci, ornare
              lectus non pellentesque et nisi ornare sodales luctus dui sodales
              lectus vulputate venenatis in pellentesque quis, hac dui ipsum nec
              eget nec morbi nec morbi lacinia leo, mauris dictumst. Molestie
              vestibulum quis, cras nunc lacinia ut. Sit dolor tempus cras
              sapien dapibus lectus platea amet, risus dapibus nec arcu mattis
              id quis, vitae adipiscing quis, velit ex. Ut. Ipsum non vitae
              vulputate dui amet, est. Et faucibus. Augue amet, amet vulputate
              nec efficitur non non non elit. Mattis ut. Id integer velit amet,
              luctus luctus ultricies. Tortor, nulla dui ultricies. Ut. Platea
              amet, habitasse dictumst. Nunc tempus lorem sit ipsum malesuada
              habitasse vel cras sit eget in dapibus morbi justo vestibulum
              aenean sit lectus dolor non ipsum est. Quam, amet, lectus id
              faucibus. In et consectetur urna et dolor nunc ornare consectetur
              amet, interdum mattis hac eleifend aenean augue vestibulum
              pulvinar imperdiet nisi habitasse ut. Non in sit nisi non nisi
              faucibus. Et faucibus. Velit ut. Nulla tortor, malesuada ornare
              lorem mattis libero, risus tortor, dictum. Sapien mollis in
              consectetur orci, dolor platea dolor dui sapien nisi nisi nulla
              mattis pellentesque in pulvinar eleifend amet ultricies. Sit
              venenatis e.
            </p>
          </BaseDD>
        </BaseAcc>
      </BaseDD>
    </BaseAcc>
  </section>

  <section class="opened" v-if="starshipsData">
    <BaseAcc title="Example with all opened dropdowns">
      <BaseDD
        v-for="ship in starshipsData"
        :key="ship.name"
        :titleDD="ship.name"
        :isAllOpened="true"
      >
        <p class="opened__content">Model: {{ ship.model }}</p>
      </BaseDD>
    </BaseAcc>
  </section>

  <section class="semi-opened" v-if="starshipsData">
    <BaseAcc title="Example with several opened dropdowns">
      <BaseDD
        v-for="(ship, index) in starshipsData"
        :key="ship.name"
        :titleDD="ship.name"
        :index="index"
        :opened="opened"
      >
        <p class="semi-opened__content">Model: {{ ship.model }}</p>
      </BaseDD>
    </BaseAcc>
  </section>
</template>

<style lang="scss" scoped>
.title {
  text-align: center;
}

.fetch {
  @include container;

  &__content {
    font-size: 0.8rem;
  }

  &__pagination {
    display: flex;
    justify-content: center;
    list-style-type: none;
    padding: 0.5rem;
  }

  &__page {
    width: 2rem;
    height: 2rem;
    border-radius: 50%;
    text-align: center;
    cursor: pointer;

    &_active {
      background: rgba(201, 201, 201, 0.5);
    }
  }
}

.nested {
  @include container;
}

.opened,
.semi-opened {
  @include container;

  &__content {
    font-size: 0.8rem;
  }
}
</style>
