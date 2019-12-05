<template>
  <div>
    <h1>Search Google Books</h1>
    <input type="text" v-model="searchText" placeholder="Python入門" />
    <p>検索タイトル: {{ searchText }}</p>
    <ul>
      <li :key="volume.id" v-for="volume in volumes">
        <a :href="volume.volumeInfo.infoLink">{{ volume.volumeInfo.title }}</a>
      </li>
    </ul>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return { searchText: "", volumes: [] };
  },
  methods: {
    async getVolumes(searchText) {
      this.volumes = await axios
        .get("https://www.googleapis.com/books/v1/volumes/?q=" + searchText)
        .then(res => {
          return res.data.items;
        })
        .catch(e => {
          error({ statusCode: 404, message: "ページが見つかりません" });
        });
    }
  },
  watch: {
    searchText: function(newSearchText, oldSearchText) {
      this.getVolumes(newSearchText);
    }
  }
};
</script>

<style></style>
