<script lang="ts">
import Vue from "vue"
import type { Video } from "@/types/Video"
import { splitDate, videos } from "@/assets/utils"

export default Vue.extend({
  data: () => ({
    groupedVideos: videos.reduce((reduced: {[key: string]: Video[]}, video: Video) => {
      (reduced[splitDate(new Date(video.publishedAt))] = reduced[splitDate(new Date(video.publishedAt))] || []).push(video)
      return reduced
    }, {})
  })
})
</script>

<template>
  <aside class="content" id="subscriptions">
    <section class="videos-container container" v-for="([date, videos], index) in Object.entries(groupedVideos)" :key="index">
      <h1>{{ date }}</h1>

      <section class="videos">
        <Video :video="video" v-for="(video, index) in videos" :key="index" />
      </section>
    </section>
  </aside>
</template>

<style lang="scss">
@import "@/assets/css/mixins.scss";

aside.content#subscriptions {
  section.videos-container {
    section.videos {
      @include grid(5, $mb: 1);
      @include breakpoint { margin: 0 -16px; gap: 0 }
      @include breakpoint(min, 1920px) { @include grid(6) }
      @include breakpoint(min, 2250px) { @include grid(6) }

      a.video footer {
        h1 {
          font-size: 14px;
          @include breakpoint { font-size: unset }
        }

        a.channel {
          display: none;
          @include breakpoint { display: block }
        }
      }
    }
  }
}
</style>