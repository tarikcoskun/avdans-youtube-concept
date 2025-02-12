<script lang="ts">
import Vue from "vue"
import { getReadableDate, findChannel, videos, groupedVideos } from "@/assets/utils"

export default Vue.extend({
  methods: { getReadableDate },

  data: () => ({
    videos,
    groupedVideos,
    channel: findChannel("jdh")
  })
})
</script>

<template>
  <aside class="content" id="explore">
    <section class="tags container">
      <button><Icon name="trending" /> <h1>Trending</h1></button>
      <button><Icon name="music" /> <h1>Music</h1></button>
      <button><Icon name="gaming" /> <h1>Gaming</h1></button>
      <button><Icon name="sports" /> <h1>Sports</h1></button>
      <button><Icon name="live" /> <h1>Live</h1></button>
      <button><Icon name="news" /> <h1>News</h1></button>
    </section>

    <section class="recommended-channel container">
      <header>
        <NuxtLink :to="`/channel/${channel.id}`" class="channel">
          <SmartImage :src="channel.avatar" width="36" height="36" radius="rounded" />
          <aside>
            <h1>{{ channel.name }}</h1>
            <span>Recommended channel for you</span>
          </aside>
        </NuxtLink>
        <button class="subscribe">Subscribe</button>
      </header>

      <section class="videos">
        <Video :video="video" v-for="(video, index) in groupedVideos[channel.name]" :key="index" />
      </section>
    </section>

    <section class="videos-container container">
      <h1>Trending Videos</h1>

      <section class="videos">
        <Video :video="video" v-for="(video, index) in videos" :key="index" />
      </section>
    </section>
  </aside>
</template>

<style lang="scss">
@import "@/assets/css/mixins.scss";

aside.content#explore {
  section {
    &.tags {
      @include grid(6, 12px, 3);
      @include breakpoint {
        overflow-x: auto;
        @include flex($gap: 16px);
        &::-webkit-scrollbar { display: none }
      }

      button {
        padding: 16px;
        cursor: pointer;
        border-radius: 8px;
        background: var(--gray);
        @include flex(center, $dir: column, $gap: 12px);
        @include breakpoint { flex: 0 0 32% }

        svg {
          width: 32px;
          &.trending path { fill: var(--red) }
          &.music path { fill: #a855f7 }
          &.gaming path { fill: #06b6d4 }
          &.sports path { fill: #f59e0b }
          &.live path, circle { fill: #10b981 }
          &.news path { fill: #ef4444 }
        }

        &:hover { background: var(--active) }
        h1 { font-size: 18px; font-weight: 500 }
      }
    }

    &.recommended-channel {
      gap: 24px;
      header {
        @include flex(center, space-between, $gap: 16px);

        a.channel {
          @include flex(center, $gap: 16px);
          @include breakpoint { align-items: flex-start }

          figure.image { flex-shrink: 0 }

          aside {
            @include flex(flex-end, $gap: 8px);
            @include breakpoint { align-items: flex-start; flex-direction: column }
            h1 { font-size: 20px; font-weight: 500; line-height: 20px; @include breakpoint { line-height: auto } }
            span { color: var(--icon); font-size: 14px }
          }
        }
      }

      section.videos {
        @include grid(4);
        @include breakpoint {
          margin: 0 -16px; padding: 0 16px;
          @include flex($gap: 16px); overflow-x: auto;
          &::-webkit-scrollbar { display: none }
        }

        @include breakpoint(min, 1920px) { @include grid(5) }
        @include breakpoint(min, 2250px) { @include grid(6) }

        a.video {
          @include breakpoint { flex: 0 0 44%; gap: 16px }
          footer {
            padding: 0;
            a.channel, a.channel-name { display: none }
          }
        }
      }
    }

    &.videos-container section.videos {
      @include grid(4, $mb: 1);
      @include breakpoint { margin: 0 -16px; gap: 0 }
      @include breakpoint(min, 1920px) { @include grid(5) }
      @include breakpoint(min, 2250px) { @include grid(6) }
    }
  }
}
</style>