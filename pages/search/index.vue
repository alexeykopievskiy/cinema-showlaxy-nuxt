<template>
  <main class="showlaxy-main showlaxy-main--page-list">
    <div class="showlaxy__container showlaxy__container--white showlaxy__container--list">
      <section class="showlaxy-main-block grid grid-column">
        <h2 class="showlaxy-main-block__header">{{title}}</h2>
        <div class="showlaxy-main-block__container grid grid-wrap" v-if="videos">
          <a
            @click.prevent="openVideo(video.url)"
            v-for="video in videos"
            :key="video.id"
            class="showlaxy-main-block__item"
            href="#"
          >
            <div class="showlaxy-main-block__parent">
              <img class="showlaxy-main-block__img" :src="video.image" alt />
            </div>
            <h3 class="showlaxy-main-block__title">{{video.title}}</h3>
            <p class="showlaxy-main-block__content">
              <span class="showlaxy-main-block__elem">{{video.year}}</span>
              <span class="showlaxy-main-block__elem" v-if="video.category">{{video.category[0]}}</span>
            </p>
          </a>
        </div>
        <button
          v-if="loadBtn"
          class="showlaxy-main-block__btn btn btn-default"
          @click="loadMore()"
          type="button"
        >Загрузить еще</button>
      </section>
    </div>
  </main>
</template>

<script>
export default {
  data: () => {
    return {
      loadBtn: true,
      videos: null,
      page: null,
      page_count: null,
      title: "Расширенный поиск"
    };
  },
  methods: {
    openVideo(i) {
      this.$router.push(i);
    },
    async loadMore() {
      if (this.page != this.page_count) {
        const symbol = this.$route.fullPath.slice(-1) == "/" ? "?" : "&";
        const { data, page } = await this.$axios.$get(
          "https://api.videout.ru" +
            this.$route.fullPath +
            symbol +
            "limit=24&page=" +
            ++this.page
        );

        if (this.page_count == page) this.loadBtn = false;
        this.videos = this.videos.concat(data);

        return {
          page
        };
      }
    }
  },
  async asyncData({ $axios, params, route }) {
    if (route.fullPath.includes("query")) {
      const query = "/search/ajax/?q=" + route.query.query;
      const data = await $axios.$get("https://api.videout.ru" + query);

      return {
        videos: data,
        loadBtn: false
      };
    } else {
      const { data, page, page_count, title } = await $axios.$get(
        "https://api.videout.ru/" + route.fullPath
      );
      if (page_count == 1) this.loadBtn = false;
      return {
        videos: data,
        page,
        page_count,
        title
      };
    }
  },
  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: "description",
          name: "description",
          content: "showlaxy - лучшие фильмы в формате HD"
        }
      ]
    };
  },
  watch: {
    async $route() {
      if (this.$route.fullPath.includes("query")) {
        const query = "/search/ajax/?q=" + this.$route.query.query;
        const data = await this.$axios.$get("https://api.videout.ru" + query);
        this.videos = data;

        this.page = 1;
        this.loadBtn = false;
      } else {
        const query = this.$route.fullPath;
        const { data, page, title, page_count } = await this.$axios.$get(
          "https://api.videout.ru" + query
        );

        this.videos = data;

        this.page = 1;
        this.loadBtn = true;

        return {
          page_count
        };
      }
    }
  }
};
</script>