<template>
  <!-- メインコンテンツ -->
  <div class="p-content">
    <div class="p-content_inner">
      <!-- メインセクション -->
      <div class="p-content_main">
        <!-- パンクず -->
        <div class="p-pankuzu">
          <NuxtLink to="/">moolike</NuxtLink><span> > </span>
          <NuxtLink :to="`/category/${query}`" class="p-pankuzu_inactive">{{categoryttl}}</NuxtLink>
        </div>
        <!-- パンクず -->
        <h1 class="p-content_subTtl">カテゴリ検索: {{categoryttl}}</h1>
        <div class="p-material">
          <v-lazy v-model="isActive" :options="{threshold: .3}" transition="fade-transition"
            v-for="(movie) in getMovies" :key="movie.id">
            <v-card class="p-material_item mx-auto my-12" max-width="250" min-width="150">
              <div class="p-material_inner p-material_inner--mv">
              <NuxtLink :to="`/detail/${movie.id}`">
                         <span class="p-material_new"
                      v-if="today - movie.created.toDate().getTime() <= 24*24*60*60*1000">new</span>
                <img :src="`http://www.moolike-stock.com/wp-content/themes/moolike_wp/thumb/${movie.mv_id}.jpg`"
                  class="p-material_thumb">
              </NuxtLink>
                </div>
              <div class="p-material_inner">
                <NuxtLink :to="`/detail/${movie.id}`">
                  <div class="p-material_innerWrap">
                    <h3 class="p-material_ttl">{{movie.name}}</h3>
                  </div>
                </NuxtLink>
                <div class="p-material_iconWrap">
                  <!-- ダウンロードリンク -->
                  <v-icon class="p-material_tag_icon" @click="downloadByUrlMp4(movie.mv_id,movie.id,movie.dl_count)">
                    mdi-download</v-icon>
                  <!-- /ダウンロードリンク -->
                </div>
              </div>
            </v-card>
          </v-lazy>
        </div>
        <div class="text-center">
          <v-pagination v-model="current_page" :length="getPageCount" class="p-page" circle></v-pagination>
        </div>
      </div>
      <!-- サイドバー -->
      <aside class="p-sideber">
        <sidebar />
      </aside>
    </div>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
        name: '',
        desc: '',
        yt_id: '',
        mv_id: '',
        length: '',
        category: '',
        tag1: '',
        tag2: '',
        tag3: '',
        movie_data: [],
        movie_serach: [],
        query: '',
        categoryttl: '',
        page: 1,
        week: 7 * 24 * 60 * 60 * 1000,
        current_page: 1,
        input: '',
        today : new Date(),
        parPage: 15,
        tags: [{
            tag: '手書き風',
            param: 'handfree'
          },
          {
            tag: 'フィルター',
            param: 'filter'
          },
          {
            tag: '日常',
            param: 'days'
          },
          {
            tag: 'カウントダウン',
            param: 'countdown'
          },
        ],
             categories: [{
            "id": "youtube",
            "name": "youtube",
          },
          {
            "id": "wedding",
            "name": "結婚式"
          },
          {
            "id": "smartphone",
            "name": "スマホサイズ"
          },
          {
            "id": "business",
            "name": "ビジネス"
          },
        ],    
      }
    },
    created: function () {
      let self = this
      this.setQuery()
      this.$store.dispatch('movies/init');
      this.$store.dispatch('movies/getCategory', this.query).then(querySnapshot => {
        querySnapshot.forEach(doc => {
          self.movie_serach.push(doc.data());
        });
      })
      this.setCategories();
    },
    methods: {
      remove(id) {
        this.$store.dispatch('movies/remove', id)
      },
      setQuery() {
        this.query = this.$route.params.category || ''
      },
      setCategories() {
        this.categories.forEach((category) => {
          if (category.id == this.query) {
            this.categoryttl = category.name
          }
        })
      },
    },
    computed: {
      movies() {
        return this.$store.state.movies.movies;
      },
      limitCount() {
        return this.movie_serach.slice(0, 3)
      },
       getMovies() {
        let current = this.current_page * this.parPage;
        let start = current - this.parPage;
        return this.movie_serach.slice(start, current)
      },      
      getPageCount() {
        return Math.ceil(this.movie_serach.length / this.parPage)
      }
    },
    watch: {
      search() {
        this.current_page = 1
      }
    }
  }

</script>

<style>
  .c-primary_btn {
    background-color: #1867c0 !important;
    margin-top: 1rem;
    margin-right: 2rem;
    text-align: right;
  }

  .c-btn {
    color: #000 !important;
    border: 1px solid #000;
    background-color: #fff !important;
    background: #fff !important;
  }

</style>
