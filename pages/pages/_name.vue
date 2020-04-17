<template>
  <main class="showlaxy-main">
    <div class="showlaxy__container grid grid-column showlaxy__container--contacts">
      <section class="showlaxy__container--white showlaxy-contacts__block">
        <h2 class="showlaxy-contacts__header">{{name}}</h2>
        <div class="showlaxy-contacts__content" v-html="content"></div>
      </section>
    </div>
  </main>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    const { name, content } = await $axios.$get(
      "https://api.videout.ru/pages/" + params.name
    );
    return { name, content };
  },
  head() {
    return {
      title: this.name,
      meta: [
        {
          hid: "description",
          name: "description",
          content: "showlaxy - лучшие фильмы в формате HD"
        }
      ]
    };
  }
};
</script>