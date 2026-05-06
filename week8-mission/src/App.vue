<script setup>
  import { ref, computed } from 'vue';
  import MovieCard from './components/MovieCard.vue';
  import AddMovieForm from './components/AddMovieForm.vue';

  const movies = ref([
    { id: 1, title: '인셉션',     rating: 9.5, likes: 0, poster: 'https://picsum.photos/seed/inception/300/450' },
    { id: 2, title: '어바웃타임', rating: 9.2, likes: 0, poster: 'https://picsum.photos/seed/abouttime/300/450' },
    { id: 3, title: '다크 나이트', rating: 9.2, likes: 0, poster: 'https://picsum.photos/seed/darknight/300/450' },
    { id: 4, title: '기생충',     rating: 8.9, likes: 0, poster: 'https://picsum.photos/seed/parasite/300/450' },
  ]);


  const sortKey = ref('rating');
  const confirmTarget = ref(null);
  const showAddForm = ref(false);

  const sortedMovies = computed(() => {
    return [...movies.value].sort((a, b) => b[sortKey.value] - a[sortKey.value]);
  });

  const handleLike = (targetId) => {
    const movie = movies.value.find(m => m.id === targetId);
    if (movie) movie.likes++;
  };

  const handleDeleteRequest = (targetId) => {
    confirmTarget.value = targetId;
  };

  const confirmDelete = () => {
    movies.value = movies.value.filter(m => m.id !== confirmTarget.value);
    confirmTarget.value = null;
  };

  const cancelDelete = () => {
    confirmTarget.value = null;
  };

  const confirmTargetTitle = computed(() => {
    const movie = movies.value.find(m => m.id === confirmTarget.value);
    return movie ? movie.title : '';
  });

  const handleAddMovie = (movieData) => {
    movies.value.push({
      id: Date.now(),
      likes: 0,
      poster: `https://picsum.photos/seed/${Date.now()}/300/450`,
      ...movieData,
    });
    showAddForm.value = false;
  };
</script>

<template>
  <div class="container">
    <header class="page-header">
      <h2>상호작용이 추가된 영화 리스트 (Props & Emit)</h2>
      <div class="header-actions">
        <button v-if="!showAddForm" class="add-btn" @click="showAddForm = true">
          + 영화 추가
        </button>
      </div>
    </header>

    <Transition name="slide">
      <AddMovieForm
        v-if="showAddForm"
        @add-movie="handleAddMovie"
        @cancel="showAddForm = false"
      />
    </Transition>

    <div class="sort-bar">
      <button
        class="sort-btn"
        :class="{ active: sortKey === 'rating' }"
        @click="sortKey = 'rating'"
      >
        ⭐ 평점 높은 순 ⭐
      </button>
      <button
        class="sort-btn"
        :class="{ active: sortKey === 'likes' }"
        @click="sortKey = 'likes'"
      >
        ❤️ 좋아요 많은 순 ❤️
      </button>
    </div>

    <div v-if="movies.length === 0" class="empty-state">
      <p class="empty-icon">🎬</p>
      <p class="empty-title">영화가 없습니다</p>
      <p class="empty-desc">상단의 '영화 추가' 버튼으로 첫 번째 영화를 등록해보세요.</p>
    </div>

    <main v-else class="movie-grid">
      <MovieCard
        v-for="m in sortedMovies"
        :key="m.id"
        :movie="m"
        @like-movie="handleLike"
        @delete-movie="handleDeleteRequest"
      />
    </main>

    <Transition name="fade">
      <div v-if="confirmTarget !== null" class="overlay" @click.self="cancelDelete">
        <div class="dialog">
          <p class="dialog-title">영화 삭제</p>
          <p class="dialog-desc">
            <strong>{{ confirmTargetTitle }}</strong>을(를) 삭제할까요?<br />
            이 작업은 되돌릴 수 없습니다.
          </p>
          <div class="dialog-actions">
            <button class="dialog-btn cancel-btn" @click="cancelDelete">취소</button>
            <button class="dialog-btn confirm-btn" @click="confirmDelete">삭제</button>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
  :root {
    --bg: #0f1117;
    --surface: #1a1d27;
    --border: #2a2d3e;
    --text-primary: #e8eaf0;
    --text-secondary: #8b8fa8;
    --accent: #5c6bc0;
    --accent-hover: #7986cb;
    --danger: #ef5350;
  }

  .container {
    padding: 40px;
    font-family: sans-serif;
    max-width: 1000px;
    margin: 0 auto;
    background-color: var(--bg);
    min-height: 100vh;
    color: var(--text-primary);
  }

  .page-header {
    display: flex;
    flex-direction: column;
    align-items: stretch;
    margin-bottom: 24px;
  }
  h2 {
    text-align: center;
    color: var(--text-primary);
    font-weight: 800;
    font-size: 1.5rem;
    margin-bottom: 12px;
  }

  .header-actions {
    display: flex;
    justify-content: flex-end;
  }
  .add-btn {
    padding: 10px 20px;
    background: #5c6bc0;
    color: #ffffff;
    border: 2px solid #9fa8da;
    border-radius: 8px;
    font-size: 0.9rem;
    font-weight: 700;
    cursor: pointer;
    transition: background 0.2s, border-color 0.2s;
  }
  .add-btn:hover {
    background: #7986cb;
    border-color: #c5cae9;
  }

  /* 정렬 버튼 */
  .sort-bar {
    display: flex;
    gap: 10px;
    justify-content: center;
    margin-bottom: 30px;
  }
  .sort-btn {
    padding: 10px 24px;
    border: 1.5px solid var(--accent);
    border-radius: 50px;
    background: transparent;
    color: var(--accent);
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s ease;
  }
  .sort-btn:hover {
    background: rgba(92, 107, 192, 0.15);
  }
  .sort-btn.active {
    background: var(--accent);
    color: #111111;
    font-weight: 800;
    box-shadow: 0 4px 14px rgba(92, 107, 192, 0.35);
  }

  .movie-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
    gap: 25px;
  }

  .empty-state {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 80px 0;
    color: var(--text-secondary);
  }
  .empty-icon  { font-size: 3.5rem; margin-bottom: 16px; }
  .empty-title { font-size: 1.1rem; font-weight: 700; color: var(--text-primary); margin-bottom: 8px; }
  .empty-desc  { font-size: 0.88rem; }

  /* 오버레이: 뒷 배경을 충분히 어둡게 해 다이얼로그 카드가 떠 보이게 한다. */
  .overlay {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.75);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 100;
  }
  .dialog {
    background: #ffffff;
    border-radius: 16px;
    padding: 32px 28px 24px;
    width: 320px;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.6);
    text-align: center;
  }
  .dialog-title {
    font-size: 1.1rem;
    font-weight: 800;
    color: #1a1d27;
    margin-bottom: 12px;
  }
  .dialog-desc {
    font-size: 0.9rem;
    color: #4a4d5e;
    line-height: 1.6;
    margin-bottom: 24px;
  }
  .dialog-desc strong {
    color: #1a1d27;
  }
  .dialog-actions {
    display: flex;
    gap: 10px;
  }
  .dialog-btn {
    flex: 1;
    padding: 11px 0;
    border: none;
    border-radius: 8px;
    font-size: 0.9rem;
    font-weight: 700;
    cursor: pointer;
    transition: opacity 0.2s;
  }
  .dialog-btn:hover { opacity: 0.85; }

  .cancel-btn {
    background: #f1f2f6;
    color: #4a4d5e;
  }
  .confirm-btn {
    background: #f1f2f6;
    color: var(--danger);
  }

  .slide-enter-active, .slide-leave-active { transition: all 0.25s ease; }
  .slide-enter-from,  .slide-leave-to      { opacity: 0; transform: translateY(-10px); }

  .fade-enter-active, .fade-leave-active { transition: opacity 0.2s ease; }
  .fade-enter-from,  .fade-leave-to      { opacity: 0; }
</style>