<template>
  <div class="news">
    <h1 class="news__title">#fakeNews</h1>
    <div class="news__filter">
      <select @change="filterAuthors" ref="filter">
        <option value="0" disabled selected>Selecione um autor</option>
        <option value="2">PJ Media</option>
        <option value="3">Charles Nisz</option>
        <option value="4">Benjamin Hirsch</option>
      </select>
      <button @click="resetFilter" ref="btnFilter" disabled>
        Limpar Filtro
      </button>
    </div>
    <div v-if="allNews && allNews.length">
       <CardNews :news="allNews"/>
    </div>
    <div v-else-if="filterNews && filterNews.length">
      <CardNews :news="filterNews"/>
    </div>
    <Loading v-else/>
  </div>
</template>

<script>
import Loading from '@/components/Loading.vue';
import CardNews from '@/components/CardNews.vue';

export default {
  name: "ImpactNews",
  components: {
    Loading,
    CardNews,
  },
  data() {
    return {
      allNews: null,
      filterNews: null,
    };
  },
  methods: {
    async getNewsData(subject) {
      const data = await fetch(
        `https://newsapi.org/v2/everything?q=${subject}&apiKey=9c2054ef6b12401eb216d0620ff3b3e1`
      );
      const response = await data.json();
      const { articles } = response;
      this.allNews = articles;
    },
    filterAuthors(e) {
      if (e.target.value === "2") {
        this.handleFilter("PJ Media");
      }

      if (e.target.value === "3") {
        this.handleFilter("Charles Nisz");
      }

      if (e.target.value === "4") {
        this.handleFilter("Benjamin Hirsch");
      }
    },
    handleFilter(author) {
      const filterNews = this.allNews.filter(
        (article) => article.author === author
      );
      this.allNews = null;
      this.filterNews = filterNews;
      this.$refs.filter.disabled = true;
      this.$refs.btnFilter.disabled = false;
    },
    resetFilter() {
      this.getNewsData("fakenews");
      this.$refs.filter.disabled = false;
      this.$refs.btnFilter.disabled = true;
    },
  },
  created() {
    this.getNewsData("fakenews");
  },
};
</script>

<style lang="scss" scoped>
.news {
  padding: 0 clamp(1rem, 4vw, 6rem) 2rem;

  &__title {
    font-size: 3rem;
    text-align: center;
    padding: clamp(2rem, 4vw, 4rem) 0;

    &::before,
    &::after {
      display: block;
      content: "";
      width: 100%;
      height: 3px;
      background-color: currentColor;
    }
  }

  &__filter {
    display: flex;
    gap: 0.25rem;
    justify-content: flex-end;
    margin-bottom: 2rem;

    select {
      border-radius: 4px;
      min-width: 200px;

      @media (max-width: 768px) {
        flex: 1;
      }
    }

    button {
      padding: 0.5rem 1rem;
      background-color: transparent;
      border: 1px solid var(--gray);
      border-radius: 4px;
      transition: 0.1s;

      &:hover {
        background-color: var(--gray);
        color: var(--light-gray);
      }

      &:disabled {
        border-color: #bdbdbd;
        color: #bdbdbd;

        &:hover {
          background-color: transparent;
        }
      }
    }
  }
}
</style>