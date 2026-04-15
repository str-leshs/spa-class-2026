<script setup>
import PageHeader from './components/PageHeader.vue'
import MovieCard from './components/MovieCard.vue'

const movies = [
  {
    id: 1,
    title: '인셉션',
    rating: 9.5,
    poster: 'https://picsum.photos/seed/inception/300/450',
    isHot: true,
    isNew: false,
    tags: ['SF', '명작']
  },
  {
    id: 2,
    title: '아바타',
    rating: 9.2,
    poster: 'https://picsum.photos/seed/avatar/300/450',
    isHot: false,
    isNew: false,
    tags: ['판타지', '대명작']
  },
  {
    id: 3,
    title: '더 배트맨',
    rating: 9.0,
    poster: 'https://picsum.photos/seed/batman/300/450',
    isHot: true,
    isNew: true,
    tags: ['느와르', '액션']
  },
  {
    id: 4,
    title: '기생충',
    rating: 8.9,
    poster: 'https://picsum.photos/seed/parasite/300/450',
    isHot: false,
    isNew: true,
    tags: ['스릴러', '명작']
  }
]
</script>

<template>
  <div class="container">
    <PageHeader title="내가 좋아하는 영화 카드" :count="movies.length" />

    <main class="movie-grid">
      <MovieCard
        v-for="movie in movies"
        :key="movie.id"
        :movie="movie"
      />
    </main>
  </div>
</template>

<style scoped>
.container { 
  padding: 50px; 
  max-width: 1100px; 
  margin: 0 auto; 
  background-color: #f1f2f6; 
  min-height: 100vh; 
}

.movie-grid { 
  display: grid; 
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); 
  gap: 35px; 
}
</style>