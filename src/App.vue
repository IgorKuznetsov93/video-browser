<template>
    <div class="container">
        <SearchBar @termChange="onTermChange"> </SearchBar>
        <div class="row">
            <VideoDetail v-if="selectedVideo" :video="selectedVideo"> </VideoDetail>
            <VideoList v-bind:videos="videos" @videoSelect="onVideoSelect"></VideoList>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar.vue';
import VideoList from './components/VideoList.vue';
import VideoDetail from './components/VideoDetail.vue';

const API_KEY = 'AIzaSyBnCeJuAFO4WPLEtEdzUzMUncCuTYTZx4w';

export default {
  name: 'App',
  components: { SearchBar, VideoList, VideoDetail },
  methods: {
    async onTermChange(searchTerm) {
      try {
        const { data } = await axios.get('https://www.googleapis.com/youtube/v3/search', {
          params: {
            key: API_KEY,
            type: 'video',
            part: 'snippet',
            q: searchTerm,
          },
        });
        this.videos = data.items;
      } catch (e) {
        console.log(e);
      }
    },
    onVideoSelect(video) {
      this.selectedVideo = video;
    },
  },
  data() {
    return {
      videos: [],
      selectedVideo: null,
    };
  },
};
</script>
