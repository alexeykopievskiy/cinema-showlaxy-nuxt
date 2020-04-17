<template>
  <main class="showlaxy-main">
    <section class="showlaxy-main-slider owl-carousel" v-if="posters">
      <div
        @click="openVideo(item.url)"
        v-for="item in posters"
        :key="item.id"
        class="showlaxy-main-slide"
      >
        <img :src="item.image" alt />
        <div class="showlaxy-main-slide__content">
          <p class="showlaxy-main-slide__label">
            <span class="showlaxy-main-slide__text">{{item.year}}</span>
            <span class="showlaxy-main-slide__text">{{item.category.join(', ')}}</span>
          </p>
          <h3 class="showlaxy-main-slide__title">{{item.title}}</h3>
        </div>
      </div>
    </section>
    <div class="showlaxy-main-slide__controls">
      <button class="showlaxy-main-slide__prev"></button>
      <button class="showlaxy-main-slide__next"></button>
    </div>
    <div class="showlaxy__container showlaxy__container--white showlaxy__container--main">
      <section class="showlaxy-main-block">
        <h2 class="showlaxy-main-block__header">Фильмы</h2>
        <div
          class="showlaxy-main-block__container showlaxy-main-block__container-carousel--default owl-carousel"
          v-if="films"
        >
          <a
            @click.prevent="openVideo(item.url)"
            v-for="item in films"
            :key="item.id"
            class="showlaxy-main-block__item"
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
      </section>
      <section class="showlaxy-main-block showlaxy-main-block__carousel">
        <h2 class="showlaxy-main-block__header">РЕКОМЕНДУЕМ ПОСМОТРЕТЬ</h2>
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
      </section>
      <section class="showlaxy-main-block">
        <h2 class="showlaxy-main-block__header">Мультфильмы</h2>
        <p class="showlaxy-main-block__subtitle">Коллекция лучших мультфильмов для детей и родителей</p>
        <div
          class="showlaxy-main-block__container showlaxy-main-block__container-carousel--default owl-carousel"
          v-if="cartoons"
        >
          <a
            @click.prevent="openVideo(item.url)"
            v-for="item in cartoons"
            :key="item.id"
            class="showlaxy-main-block__item"
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
      </section>
      <section class="showlaxy-main-block">
        <h2 class="showlaxy-main-block__header">Новости</h2>
        <div
          class="showlaxy-main-block__container showlaxy-main-block__container-carousel showlaxy-main-block__container-carousel--news owl-carousel"
          v-if="news"
        >
          <a
            @click.prevent="openNews(item.url)"
            v-for="item of news"
            :key="item.id"
            class="showlaxy-main-block__item showlaxy-main-block__item--lg"
            href="#"
          >
            <img class="showlaxy-main-block__img" :src="item.image" alt />
            <p class="showlaxy-main-block__text">{{item.title}}</p>
          </a>
        </div>
      </section>
    </div>
  </main>
</template>

<script>
let requestUrl = "https://api.videout.ru";

export default {
  data: () => {
    return {
      source: null,
      films: null,
      cartoons: null,
      news: null,
      posters: null
    };
  },
  mounted() {
    $(".showlaxy-main-slider").owlCarousel({
      center: true,
      items: 2,
      loop: true,
      margin: 10,
      autoWidth: true
    });
    $(".showlaxy-main-slide__next").click(function() {
      $(".showlaxy-main-slider").trigger("next.owl.carousel", [300]);
    });
    $(".showlaxy-main-slide__prev").click(function() {
      $(".showlaxy-main-slider").trigger("prev.owl.carousel", [300]);
    });

    $(".showlaxy-main-block__container-carousel").owlCarousel({
      items: 4,
      loop: true,
      margin: 26,
      autoWidth: true
    });

    $(".showlaxy-main-block__container-carousel--default").owlCarousel({
      items: 6,
      loop: true,
      margin: 17,
      autoWidth: true,
      autoplay: true,
      autoplayTimeout: 2000,
      autoplayHoverPause: true
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
  methods: {
    openVideo(i) {
      this.$router.push(i);
    },
    openNews(i) {
      this.$router.push(i);
    }
  },
  async asyncData({ $axios, params }) {
    let response = await $axios.get(requestUrl);
    const { recommendations, movies, cartoons, news, posters } = response.data;
    return {
      recommendations,
      films: movies,
      cartoons,
      news,
      posters
    };
  }
};
</script>

<style lang="scss">
.showlaxy-main {
  &-slide {
    &__prev {
      margin-left: 80px;
      background: transparent url("../assets/img/arrow.png") no-repeat;
      width: 27px;
      height: 42px;
      border: none;
      outline: none;
      cursor: pointer;

    }
    &__next {
      margin-right: 80px;
      margin-left: 80px;
      background: transparent url("../assets/img/arrow.png") no-repeat;
      width: 27px;
      height: 42px;
      border: none;
      outline: none;
      transform: rotate(-180deg);
      cursor: pointer;
    }
  }
  &-block {
    &__carousel {
      &-left {
        width: 27px;
        height: 42px;
        margin-left: 24px;
        transform: rotate(-180deg);
        border: none;
        outline: none;
        cursor: pointer;
        position: relative;
        z-index: 100;
      }
      &-right {
        width: 27px;
        height: 42px;
        margin-right: 24px;
        border: none;
        outline: none;
        cursor: pointer;
        position: relative;
        z-index: 100;
      }
    }
  }
}
.showlaxy-main-block__container-carousel > span {
  position: absolute;
  top: 50%;
  left: 0;
}

.owl-carousel.owl-theme + span {
  left: initial;
  right: 0;
}

.owl-carousel.owl-loaded {
  display: flex !important;
}

.showlaxy-main-block__img {
  width: 100%;
  height: auto;
}

.showlaxy-main-block__container {
  width: 95%;
}
.showlaxy-main-slider.owl-carousel .owl-item img {
  height: 100%;
  min-width: 1330px;
}

.showlaxy-main-slide {
  height: 479px;
}

.showlaxy-main-block__container-carousel {
  .showlaxy-main-block__item--lg .showlaxy-main-block__img {
    height: 400px;
  }
}

.showlaxy-main-block__container-carousel--news {
  .showlaxy-main-block__item--lg .showlaxy-main-block__img {
    width: auto;
    height: 284px;
  }
}

.showlaxy-main-block__item--lg {
  overflow: hidden;

  .showlaxy-main-block__img {
    width: auto;
  }
}
</style>
