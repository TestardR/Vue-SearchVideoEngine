<template>
  <div class="container">
    <SearchBar @termChange="onTermChange"></SearchBar>
    <VideoDetail :video="selectedVideo" />
    <VideoList :videos="videos" @videoSelect="onVideoSelect"></VideoList>
  </div>
</template>

<script>
import axios from 'axios';

import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';

export default {
  name: 'App',
  components: {
    SearchBar,
    VideoList,
    VideoDetail,
  },
  data() {
    return { videos: [], selectedVideo: null };
  },
  methods: {
    async onTermChange(searchTerm) {
      try {
        const result = await axios.get(
          'https://www.googleapis.com/youtube/v3/search',
          {
            params: {
              key: process.env.VUE_APP_API_KEY,
              type: 'video',
              part: 'snippet',
              q: searchTerm,
            },
          }
        );
        this.videos = result.data.items;
      } catch (error) {
        console.error(error);
      }
    },
    onVideoSelect(video) {
      this.selectedVideo = video;
    },
  },
};
</script>

<style></style>
