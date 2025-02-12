<template>
  <main class="Container">
    <Cover v-if="app && app.cover && app.cover.value" :img="app.cover.value" />
    <div class="Inner">
      <div class="Categories">
        <div
          v-for="category in categories"
          :key="category._id"
          class="Category"
        >
          <NuxtLink :to="`/category/${category.slug}`" class="Category_Link">
            <em>{{ category.emoji.value }}</em>
            <div class="Category_Text">
              <h2>{{ category.name }}</h2>
              <p>{{ category.description }}</p>
            </div>
          </NuxtLink>
        </div>
      </div>
      <div class="Articles">
        <h2 class="Articles_Heading">Recent articles</h2>
        <div class="Articles_Inner">
          <ArticleCard
            v-for="article in articles"
            :key="article._id"
            :article="article"
          />
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import { mapGetters } from 'vuex'
import { getSiteName } from 'utils/head'

export default {
  async asyncData({ $config, store }) {
    await store.dispatch('fetchApp', $config)
    await store.dispatch('fetchArticles', $config)
    await store.dispatch('fetchCategories', $config)
    return {}
  },
  head() {
    return {
      title: getSiteName(this.app),
    }
  },
  computed: {
    ...mapGetters(['app', 'articles', 'categories']),
  },
}
</script>

<style scoped>
.Inner {
  margin: 0 auto;
}
.Category_Link {
  color: #333;
  text-decoration: none;
  display: flex;
  align-items: center;
  padding: 20px 24px 20px 20px;
  min-width: 0;
  position: relative;
  transition: background 0.2s;
}
.Category_Link::after {
  content: '';
  width: calc(100% - 78px);
  height: 1px;
  background: #e5e5e5;
  position: absolute;
  left: 78px;
  bottom: 0;
}
.Category_Link:hover {
  background: #f8f8f8;
}
.Category_Link:active {
  background: none;
  transition: none;
}
.Category_Text {
  width: calc(100% - 54px);
}
.Category_Link > em {
  font-size: 4rem;
  font-style: normal;
  line-height: 1;
  min-width: 54px;
}
.Category_Text > h2 {
  margin: 0 0 4px 0;
  padding: 0;
  font-size: 1.4rem;
  line-height: 1.5;
}
.Category_Text > p {
  margin: 0;
  padding: 0;
  font-size: 1.2rem;
  line-height: 1.5;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.Articles {
  padding: 24px 24px 40px 24px;
  margin: 0 auto;
}
.Articles_Heading {
  font-weight: bold;
  font-size: 1.8rem;
  margin: 0 0 16px 0;
}
@media (min-width: 600px) {
  .Inner {
    padding: 60px;
  }
  .Categories {
    display: flex;
    flex-wrap: wrap;
    margin: 0 0 32px 0;
  }
  .Category {
    width: calc(50% - 20px);
    margin: 0 40px 32px 0;
  }
  .Category:nth-child(2n) {
    margin: 0 0 32px 0;
  }
  .Category_Link {
    flex-direction: column;
    align-items: flex-start;
    padding: 0;
  }
  .Category_Link::after {
    content: none;
  }
  .Category_Link:hover {
    background: none;
  }
  .Category_Link:hover .Category_Text > h2 {
    text-decoration: underline;
  }
  .Category_Link > em {
    margin: 0 24px 12px 0;
    font-size: 4rem;
    min-width: inherit;
  }
  .Category_Text {
    width: 100%;
  }
  .Category_Text > h2 {
    font-size: 1.6rem;
    margin: 0 0 4px 0;
    line-height: inherit;
  }
  .Category_Text > p {
    font-size: 1.4rem;
    white-space: inherit;
    overflow: inherit;
    text-overflow: inherit;
    line-height: inherit;
  }
  .Articles {
    padding: 0;
  }
}
@media (min-width: 960px) {
  .Inner {
    max-width: 1024px;
  }
  .Category {
    width: calc(33.333% - 27px);
  }
  .Category:nth-child(2n) {
    margin: 0 40px 32px 0;
  }
  .Category:nth-child(3n) {
    margin: 0 0 32px 0;
  }
  .Articles_Inner {
    display: flex;
    flex-wrap: wrap;
  }
}
</style>
