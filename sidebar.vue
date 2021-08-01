<template>
  <div>
    <div>
      <h3 class="p-sidebar_ttl">カテゴリから探す</h3>
      <ul class="p-sidebar_category" v-for="category in categories" :key="category.id">
        <li class="p-sidebar_category__item">
          <NuxtLink :to="`/category/${category.id}`">
            {{category.name}}
          </NuxtLink>
          </li>
      </ul>
    </div>
    <div class="p-sidebar_contents">
      <h3 class="p-sidebar_ttl">タグから探す</h3>
      <ul class="p-sidebar_category" v-for="tag in tags" :key="tag.id">
        <li class="p-sidebar_category__item">
          <NuxtLink :to="`/tags/${tag.param}`">
            {{tag.tag}}
          </NuxtLink>
          </li>
      </ul>
    </div>
  </div>
</template>
<script>
  export default {
    data: function () {
      return {
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
        tags: [
          {
            tag: '手書き風',param:'handfree'
          },
          {
            tag: 'フィルター',param:'filter'
          },
          {
            tag: '日常',param:'days'
          },
          {
            tag: 'カウントダウン',param:'countdown'
          },
          {
            tag: 'v-log',param:'v-log'
          },
          {
            tag: 'サプライズ',param:'surprise'
          },
          {
            tag: '結婚式',param:'wedding'
          },
        ],
      }
    }
  }

</script>
<style>
.p-sidebar_ttl{
   font-weight: bold; 
   font-size: 1rem;
   margin-bottom: .8rem;
}


.p-sidebar_category__item{
  font-size: .8rem;
  padding: .5rem;
  padding-left: .8rem;
  margin: .3rem 0;
  position: relative;
    transition: all .3s;
}

.p-sidebar_category__item::after{
  content: '';
 position: absolute;
   width: 10px;
  height: 10px;
  border-style: solid;
  border-width: 1px;
  border-color: transparent transparent #999 #999;
  top:50%;
  right: 30px;
  transform: rotate(225deg) translateY(70%);
  transition: all .3s;
}

.p-sidebar_category__item:hover{
  background-color: #e9e9e9;
}

.p-sidebar_category__item:hover::after{
  right: 20px;
}

.p-sidebar_contents{
  margin-top: 2rem;
  position: relative;
  padding-top: 1rem;
}

.p-sidebar_contents::before{
  position: absolute;
  content: '';
  width: 100%;
  height: 0.09rem;
  top: 0px;
  background: #e9e9e9;
}

</style>