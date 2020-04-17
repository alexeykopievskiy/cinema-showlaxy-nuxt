<template>
  <main class="showlaxy-main">
    <div class="showlaxy__container grid grid-column showlaxy__container--inner">
      <section class="showlaxy__container--white showlaxy-inner__block" v-if="video">
        <div class="showlaxy-inner__video ut-overroll" style="width: 939px">
          <div>
            <h2 class="showlaxy-inner__header">{{video.title}}</h2>
            <h4 class="showlaxy-inner__subheader">{{video.subtitle}}</h4>
          </div>
          <div class="showlaxy-inner__video-inside" v-html="video.player_code"></div>
        </div>
      </section>
      <section class="showlaxy-inner__block showlaxy-inner__block-poster" v-if="video">
        <div class="showlaxy-inner__content">
          <div class="showlaxy-inner__content-poster">
            <img class="showlaxy-inner__content-img" style="width: 235px" :src="video.image" />
          </div>
          <div class="showlaxy-inner__content-description grid grid-column">
            <div class="showlaxy-inner__content-list">
              <div class="showlaxy-inner__content-item">
                <span>Год</span>
                <span>{{video.year}}</span>
              </div>
              <div class="showlaxy-inner__content-item">
                <span>Страна</span>
                <span>{{video.country}}</span>
              </div>
              <div class="showlaxy-inner__content-item">
                <span>Жанр</span>
                <span>{{video.category}}</span>
              </div>
            </div>
            <span class="showlaxy-inner__content-text">{{video.description}}</span>
          </div>
        </div>
      </section>
      <section class="showlaxy__container--white showlaxy-main-block showlaxy-inner__block-container">
        <h2 class="showlaxy-main-block__header">Новости</h2>
        <div class="showlaxy-main-block__container" v-if="news">
          <a
            @click.prevent="openNews(item.url)"
            v-for="item in news"
            :key="item.id"
            class="showlaxy-main-block__item showlaxy-main-block__item--lg"
            href="#"
          >
            <img class="showlaxy-main-block__img" :src="item.image" alt />
            <p class="showlaxy-main-block__text">{{item.title}}</p>
          </a>
        </div>
      </section>
      <section class="showlaxy__container--white showlaxy-inner__block-container">
        <div class="showlaxy-main-block showlaxy-main-block__carousel">
          <h2 class="showlaxy-main-block__header">Рекумондуем посмотреть</h2>
          <div
            class="showlaxy-main-block__container showlaxy-main-block__container-carousel owl-carousel"
          >
            <a
              @click.prevent="openVideo(item.url)"
              v-for="item in recommendations"
              :key="item.id"
              class="showlaxy-main-block__item showlaxy-main-block__item--lg"
              href="#"
            >
              <img class="showlaxy-main-block__img" :src="item.image" alt />
              <h3 class="showlaxy-main-block__title">{{item.title}}</h3>
              <p class="showlaxy-main-block__content">
                <span class="showlaxy-main-block__elem">{{item.year}}</span>
                <span class="showlaxy-main-block__elem">{{item.category.join(', ')}}</span>
              </p>
            </a>
          </div>
          <div class="showlaxy-main-block__carousel-btn">
            <button class="showlaxy-main-block__carousel-left"></button>
            <button class="showlaxy-main-block__carousel-right"></button>
          </div>
        </div>
      </section>
    </div>
  </main>
</template>

<script>
export default {
  validate({ params }) {
    return /^\d+$/.test(params.id);
  },
  data: () => {
    return {
      video: null,
      news: null,
      recommendations: null
    };
  },
  methods: {
    openNews(i) {
      this.$router.push(i);
    }
  },
  head () {
    return {
      title: this.video.title,
      meta: [
        { hid: 'description', name: 'description', content: this.video.description }
      ]
    }
  },
  mounted() {
    $(".showlaxy-main-block__container-carousel").owlCarousel({
      items: 4,
      loop: true,
      margin: 26,
      autoWidth: true
    });

    $(".showlaxy-main-block__carousel-right").click(function() {
      $(".showlaxy-main-block__container-carousel").trigger("next.owl.carousel", [
        300
      ]);
    });
    $(".showlaxy-main-block__carousel-left").click(function() {
      $(".showlaxy-main-block__container-carousel").trigger("prev.owl.carousel", [
        300
      ]);
    });
  },
  async asyncData({ $axios, params, route }) {
    const response = await $axios.$get(
      "https://api.videout.ru" + route.fullPath
    );
    const { video, news, recommendations } = await $axios.$get(
      "https://api.videout.ru" + route.fullPath
    );
    return {
      video,
      news,
      recommendations
    };
  }
};
</script>

<style lang="scss">
.showlaxy-inner__block-container {
  padding-top: 40px;
}
.showlaxy-inner__video-inside {
  width: 100%;
  max-width: 939px;
  min-height: 530px;

  iframe {
    width: 100%;
    height: 100%;
    min-height: 530px;
  }
}
</style>