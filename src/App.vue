<template>
    <Waterfall :list="list" :gutter="10" :crossOrigin="false" :width="400" :lazyload="true" :imgSelector="_lazyImg">
    <!-- v2.6.0之前版本插槽数据获取 -->
    <!-- <template #item="{ item, url, index }"> -->
    <!-- 新版插槽数据获取 -->
    <template #default="{ item, url, index }">
      <div class="card">
        <LazyImg :url="url" class="_lazyImg"/>
        <p class="text">{{item.text}}</p>
      </div>
    </template>
  </Waterfall>
  <div ref="loadMore">loadmore</div>
</template>
<script setup>
    import axios from 'axios';
    import { onMounted, reactive, ref} from 'vue';
    import { LazyImg, Waterfall } from 'vue-waterfall-plugin-next'
    import 'vue-waterfall-plugin-next/dist/style.css'
    import 'animate.css'

    const list = reactive([])

    /*
    const {data} = await axios.get(`https://www.loliapi.com/acg/?id=${i}&type=url`)

    */
   const loadMore = ref(null)
    onMounted(() => {
      loadImg()

      const io = new IntersectionObserver(([entry]) => {
          if (entry.isIntersecting) {
            loadImg()
          }
      }, { threshold: 1 })

      io.observe(loadMore.value)
    })
    

    const loadImg = async () => {
        const num = 15

        const {data} = await axios.get('https://t.alcy.cc/moez/?json&quantity='+num)

        // console.log(data)
        const urls = data.match(/https[^\s]+\.webp/g)
       
        console.log(urls)
        urls.forEach(el => {
            list.push({src: el})
        })
    }
</script>
<style>
  .lazy__img[lazy=loaded] {
    width: 100%;
    border: 1px solid pink;
}

  ._lazyImg img{
    transition: transform .3s ease-in;
    /* overflow: hidden; */
  }

  ._lazyImg:hover img{
    transform: scale(1.2);
  }
</style>