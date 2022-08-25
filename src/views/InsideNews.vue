<template>
  <main class="news" v-if="news">
    <nav>
      <router-link to="/">&#8592; Voltar</router-link>
    </nav>
    <h1 class="news__title">{{ news.title }}</h1>
    <div class="news__publication">
      <span class="news__author"><b>Autor:</b> {{ news.author }}</span>
      <span class="news__date"
        ><b>Data da publicação:</b> {{ news.publishedAt.slice(0, 10) }}</span
      >
    </div>
    <figure class="news__figure">
      <img :src="news.urlToImage" alt="" />
    </figure>
    <p>{{ news.content }}</p>
  </main>
</template>

<script>
export default {
  name: "InsideNews",
  data() {
    return {
      news: null,
    };
  },
  methods: {
    async getNewsData(article, subject) {
      const data = await fetch(
        `https://newsapi.org/v2/everything?q=${subject}&apiKey=9c2054ef6b12401eb216d0620ff3b3e1`
      );
      const response = await data.json();
      const { articles } = response;
      const news = articles[article];

      this.news = news;
    },
    showArticle() {
      const url = window.location.href;
      const pattern = /\d{1,4}$/g;
      const articleNumber = url.match(pattern);

      this.getNewsData(articleNumber, "fakenews");
    },
  },
  created() {
    this.showArticle();
  },
};
</script>

<style lang="scss" scoped>
nav {
  margin-bottom: 1rem;
}

nav a {
  font-weight: bold;
  color: var(--primary);

  &:hover {
    text-decoration: underline;
  }
}

nav a.router-link-exact-active {
  color: var(--primary);
}
.news {
  padding: 0 clamp(1rem, 4vw, 6rem) 2rem;
  max-width: 75ch;
  margin: 2rem auto;

  &__title {
    line-height: 1.2;
  }

  &__publication {
    display: grid;
    padding: 1rem 0;

    &::before,
    &::after {
      display: block;
      content: "";
      width: 100%;
      height: 1px;
      background-color: var(--primary);
      margin: 0.5rem 0;
    }
  }

  &__figure {
    img {
      width: 100%;
      aspect-ratio: 16 / 9;
      object-fit: cover;
      object-position: top center;
      margin-bottom: 2rem;
      max-height: 400px;
    }
  }
}
</style>