<script setup>
  import { ref } from 'vue';

  const emit = defineEmits(['add-movie', 'cancel']);
  const title  = ref('');
  const rating = ref('');

  // 등록 버튼 클릭 시 유효성 검사를 먼저 수행
  // 값이 비어있거나 범위를 벗어난 경우 alert()로 안내하고 등록 막기
  const handleSubmit = () => {
    if (!title.value.trim()) {
      alert('제목을 입력해주세요.');
      return;
    }

    const ratingNum = Number(rating.value);
    if (rating.value === '') {
      alert('평점을 입력해주세요.');
      return;
    }
    if (isNaN(ratingNum) || ratingNum < 0 || ratingNum > 10) {
      alert('평점은 0에서 10 사이의 숫자로 입력해주세요.');
      return;
    }

    // 유효성 검사를 통과 -> 부모에게 데이터를 전달하고 입력값 초기화
    emit('add-movie', {
      title: title.value.trim(),
      rating: Math.round(ratingNum * 10) / 10,
    });

    title.value  = '';
    rating.value = '';
  };
</script>

<template>
  <div class="form-card">
    <h3 class="form-title">신규 영화 등록</h3>

    <div class="form-group">
      <label class="form-label">제목</label>
      <input
        v-model="title"
        class="form-input"
        placeholder="영화 제목을 입력하세요"
      />
    </div>

    <div class="form-group">
      <label class="form-label">평점 (0 ~ 10)</label>
      <input
        v-model="rating"
        class="form-input"
        type="number"
        step="0.1"
        min="0"
        max="10"
        placeholder="예: 8.5"
      />
    </div>

    <div class="form-actions">
      <button class="form-btn cancel-btn" @click="$emit('cancel')">취소</button>
      <button class="form-btn submit-btn" @click="handleSubmit">등록</button>
    </div>
  </div>
</template>

<style scoped>
  .form-card {
    background: #1a1d27;
    border: 1px solid #2a2d3e;
    border-radius: 14px;
    padding: 28px;
    margin-bottom: 28px;
  }
  .form-title {
    color: #e8eaf0;
    font-size: 1rem;
    font-weight: 700;
    margin-bottom: 20px;
  }
  .form-group {
    margin-bottom: 16px;
  }
  .form-label {
    display: block;
    font-size: 0.82rem;
    font-weight: 600;
    color: #8b8fa8;
    margin-bottom: 6px;
  }
  .form-input {
    width: 100%;
    padding: 10px 14px;
    background: #12141e;
    border: 1.5px solid #2a2d3e;
    border-radius: 8px;
    color: #e8eaf0;
    font-size: 0.9rem;
    outline: none;
    transition: border-color 0.2s;
    box-sizing: border-box;
  }
  .form-input::placeholder {
    color: #8b8fa8;
    opacity: 0.6;
  }
  .form-input:focus {
    border-color: #5c6bc0;
  }
  .form-actions {
    display: flex;
    gap: 10px;
    justify-content: flex-end;
    margin-top: 8px;
  }
  .form-btn {
    padding: 10px 22px;
    border: none;
    border-radius: 8px;
    font-size: 0.88rem;
    font-weight: 700;
    cursor: pointer;
    transition: opacity 0.2s;
  }
  .form-btn:hover { opacity: 0.85; }
  .cancel-btn { background: #2a2d3e; color: #8b8fa8; }
  .submit-btn { background: #5c6bc0; color: #fff; }
</style>