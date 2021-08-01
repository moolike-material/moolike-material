<template>
  <div class="p-material">
    <v-lazy v-model="isActive" :options="{threshold: .3}" transition="fade-transition" v-for="movie of limitCount"
      :key="movie.id<6">
      <v-card class="p-material_item mx-auto my-12" max-width="250" min-width="150">
        <div class="p-material_inner p-material_inner--mv">
        <NuxtLink :to="`/detail/${movie.id}`" class="p-link">
        <span class="p-material_new" v-if="today - movie.created.toDate().getTime() <= 24*24*60*60*1000"> new</span>
        <img :src="`http://www.moolike-stock.com/wp-content/themes/moolike_wp/thumb/${movie.mv_id}.jpg`"
          class="p-material_thumb">
        </NuxtLink>
        </div>
        <div class="p-material_inner">
          <NuxtLink :to="`/detail/${movie.id}`" class="p-link">
          <div class="p-material_innerWrap">
            <h3 class="p-material_ttl">{{movie.name}}</h3>
          </div>
          </NuxtLink>
          <div class="p-material_iconWrap">
            <!-- ダウンロードリンク -->
            <v-icon class="p-material_tag_icon" @click="downloadByUrlMp4(movie.mv_id,movie.id,movie.dl_count)">
              mdi-download</v-icon>
            <!-- /ダウンロードリンク -->
            <!-- カテゴリアイコン -->
            <!-- <div class="p-material_tagWrap">
              <v-chip class="ma-2 p-material_tag" color="indigo" text-color="white" v-if="movie.category == 'youtube'">
                <v-icon class="p-material_tag_icon">mdi-movie-edit</v-icon>
                {{movie.category}}
              </v-chip>
              <v-chip class="ma-2 p-material_tag" color="indigo" text-color="white" v-if="movie.category == 'wedding'">
                <v-icon class="p-material_tag_icon">mdi-human-male-female</v-icon>
                {{movie.category}}
              </v-chip>
            </div> -->
            <!-- /カテゴリアイコン -->
          </div>
        </div>
      </v-card>
    </v-lazy>
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
        created: '',
        movie_data: [],
        page: 1,
        today: '',
        week: 7 * 24 * 60 * 60 * 1000,
      }
    },
    created: function () {
      this.$store.dispatch('movies/init');
      let date = new Date();
      this.today = date.getTime();

    },
    methods: {
      remove(id) {
        this.$store.dispatch('movies/remove', id)
      },
        downloadByUrlMp4(url,id,count) {
          let mvcount = count+1;
          let mvarr = [id,mvcount]
            this.$store.dispatch('movies/dlcount',mvarr)
            const link = document.createElement('a')
            link.download = `${url}.mp4`
            link.href = `/dl/mp4/${url}.mp4`
            link.click()
        }
    },
    computed: {
      movies() {
        // return this.$store.state.movies.movies
        return this.$store.getters['movies/orderdMovies']
      },
      limitCount(){
        return this.movies.slice(0,3)
      }
    }
  } 

</script>
