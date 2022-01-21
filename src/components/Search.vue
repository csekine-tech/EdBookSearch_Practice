<template>
  <div id="app">
    <h1>{{ msg }}</h1>
    <p>{{ message }}</p>
  </div>
</template>

<script>
import axios from "axios";
// import _ from "lodash";
import Rakuten from "../assets/RakutenApi_config";
export default {
  props: {
    msg: String,
  },
  data: () => ({
    books: null,
    keyword: "",
    message: "",
  }),
  watch: {
    // keyword: function () {
    //   this.message = "Waiting for you to stop typing...";
    //   this.debouncedGetAnswer();
    // },
  },
  mounted: function () {
    this.getAnswer();
    // this.debouncedGetAnswer = _.debounce(this.getAnswer, 1000);
  },
  methods: {
    getAnswer: function () {
      //   if (this.keyword === "") {
      //     console.log("空です");
      //     this.items = null;
      //     return;
      //   }
      this.message = "Loading...";
      const vm = this;
      const params = {
        applicationId: Rakuten.APPLICATION_ID,
        // format: "",
        application_seacret: Rakuten.APPLICATION_SEACRET,
        affiliateId: Rakuten.AFFILIATE_ID,
        keyword: "算数",
        booksGenreId: "001002",
        // isbnjan: "",
      };
      axios
        .get(
          //   "https://app.rakuten.co.jp/services/api/BooksTotal/Search/20170404?applicationId=1032781388411408202&keyword=%E6%9C%AC&booksGenreId=001002"
          Rakuten.ACCESS_URL,
          { params }
        )
        .then(function (response) {
          console.log(response);
        })
        .catch(function (error) {
          vm.message = "Error!" + error;
        })
        .finally(function () {
          vm.message = "";
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
