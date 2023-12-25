<template>
  <div id="app">
    <h1>Dudurudh RSS Feed</h1>
    <div v-for="feed in feeds" :key="feed.title">
      <h3>{{ feed.title }}</h3>
      <p>{{ feed.content }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data() {
    return {
      feeds: []
    };
  },
  mounted() {
    this.fetchRSSFeed();
  },
  methods: {
    async fetchRSSFeed() {
      const proxyURL = 'https://api.allorigins.win/get?url=';
      const rssURL = encodeURIComponent('https://poweratwork.us/thedownload/feed/rss');
      try {
        const response = await axios.get(`${proxyURL}${rssURL}`);
        const parser = new DOMParser();
        const xmlDoc = parser.parseFromString(response.data.contents, 'text/xml');
        const items = xmlDoc.getElementsByTagName('item');
        for (let i = 0; i < items.length; i++) {
          const title = items[i].getElementsByTagName('title')[0].textContent;
          const content = items[i].getElementsByTagName('description')[0].textContent;
          this.feeds.push({ title, content });
        }
      } catch (error) {
        console.error('Error fetching RSS Feed:', error);
      }
    }
  }
};
</script>

<style>
/* Add some basic styling */
</style>
