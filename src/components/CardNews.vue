<template>
  <ul class="news-card">
    <li
      class="news-card__article"
      v-for="(news, index) in news"
      :key="news.author"
    >
      <router-link
        :to="{
          name: 'InsideNews',
          params: { author: `${news.title.split(' ').join('')}-${index}` },
        }"
      >
        <figure class="news-card__figure">
          <img class="news-card__img" :src="news.urlToImage" alt="" />
          <div class="news-card__wrapper-title">
            <h2 class="news-card__title">{{ news.title }}</h2>
          </div>
        </figure>
        <p class="news-card__text">{{ news.content }}</p>
      </router-link>
    </li>
  </ul>
</template>

<script>
export default {
  name: "CardNews",
  props: ["news"],
};
</script>

<style lang="scss" scoped>
.news-card {
  display: grid;
  gap: clamp(1rem, 4vw, 2rem);
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));

  &__article {
    box-shadow: var(--shadow);
    border-radius: 4px;
    overflow: hidden;
    transition: 0.3s;

    &:hover {
      box-shadow: var(--strong-shadow);
      transform: scale(1.1);

      h2 {
        color: var(--primary);
      }

      figure {
        border-bottom: 1px solid var(--primary);
      }
    }

    a {
      display: grid;
    }
  }

  &__wrapper-title {
    padding: 0.5rem;
  }

  &__title {
    -webkit-box-orient: vertical;
    display: -webkit-box;
    -webkit-line-clamp: 3;
    overflow: hidden;
    line-height: 1.2;
    margin: .5rem 1rem;
    min-height: 90px;
  }

  &__figure {
    border-bottom: 1px solid var(--light-gray);
  }

  &__img {
    width: 100%;
    height: 200px;
    min-width: 115px;
    object-fit: cover;
    object-position: top center;
  }

  &__text {
    padding: 1rem;
  }
}
</style>