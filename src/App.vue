<template>
  <div id="app">
    <div class="hero is-white is-gradient is-bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">R&M</span>
          <span class="subtitle">Personajes</span>
        </h1>

        <button class="button is-success is-rounded" v-on:click="fetch">
          Consultar
        </button>
      </div>
    </div>
    <div
      class="columns is-desktop is-mobile is-tablet is-multiline is-centered"
    >
      <Character v-for="character of characters" vi-bind:key="character.id" v-bind:character="character"/>
    </div>
    <nav class="pagination" role="navigation" aria-label="pagination">
      <a class="pagination-previous" v-on="changePage(page - 1)">Anterior</a>
      <ul class="pagination-list">
        <a class="pagination-link is-current">{{ page }}</a>
      </ul>
      <a class="pagination-next" v-on="changePage(page + 1)">Siguiente</a>
    </nav>
  </div>
</template>
<script>
import Character from "./components/Character";
import axios from "axios";
export default {
  name: "App",
  data: function () {
    return {
      characters: [],
      page: 1,
      pages: 1,
    };
  },
  create: function () {
    this.fetch();
  },
  components: {
    Character,
  },
  methods: {
    fetch() {
      const params = {
        page: this.page,
      };
      let result = axios
        .get("https://rickandmortyapi.com/api/character/",{params})
        .then((res) => {
          console.log(res.data.results);
          this.characters = res.data.results;
          this.pages = res.data.info.pages;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    changePage(page) {
      this.page = (page <= 0 || page > this.pages ? this.page : page);
      this.fetch();
    },
  },
};
</script>

<style>
</style>
