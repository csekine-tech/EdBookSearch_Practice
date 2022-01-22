<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <input type="text" v-model="keyword" />
    <p>{{ message }}</p>
    <div v-if="books">
      <p>検索結果:{{ books.count }}</p>
      <p>ページ:{{books.page}}</p>
      <ul>
        <li v-for="book in books.Items" :key="book.isbnjan">
          <a v-bind:href="book.Item.itemUrl" target="_blank">
            {{ book.Item.title }}
          </a>
        </li>
      </ul>
      <button v-if="(books.page>1)&&(books.page<=books.pageCount)" v-on:click="toPrevPage(books.page)">prev</button>
      <button v-if="(books.page>0)&&(books.page<=books.pageCount)" v-on:click="toNextPage(books.page)">next</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import _ from "lodash";
import Rakuten from "../assets/RakutenApi_config";
export default {
  props: {
    msg: String,
  },
  data: () => ({
    books: null,
    keyword: "",
    message: "",
    page: 1,
  }),
  watch: {
    keyword: function () {
      this.message = "Waiting for you to stop typing...";
      this.debouncedGetAnswer();
    },
  },
  mounted: function () {
    this.getAnswer();
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 1000);
  },
  methods: {
    getAnswer: function (page) {
      if (this.keyword === "") {
        console.log("空です");
        this.items = null;
        return;
      }
      this.message = "Loading...";
      const vm = this;
      const params = {
        applicationId: Rakuten.APPLICATION_ID,
        application_secret: Rakuten.APPLICATION_SECRET,
        affiliateId: Rakuten.AFFILIATE_ID,
        keyword: this.keyword,
        //学習参考書のジャンルID
        booksGenreId: "001002",
        page: page,
      };
      axios
        .get(Rakuten.ACCESS_URL, { params })
        .then(function (response) {
          console.log(response);
          vm.books = response.data;
        })
        .catch(function (error) {
          vm.message = "Error!" + error;
        })
        .finally(function () {
          vm.message = "";
        });
    },
    toNextPage: function (page) {
      page = page + 1;
      this.getAnswer(page);
    },
    toPrevPage: function (page) {
      page = page - 1;
      this.getAnswer(page);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
