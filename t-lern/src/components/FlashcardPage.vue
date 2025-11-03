<template>
  <div class="page">
    <header class="topbar">
      <div class="chip">
        <button class="btn complexity" @click="markСomplexity">Категория: Простые слова</button>
      </div>
      <div class="icons">
        <button class="icon-btn" title="Статистика" @click="onStats">
          <img :src="barIcon" alt="stats" class="icon-img" />
        </button>
        <button class="icon-btn" title="Настройки" @click="onSettings">
          <img :src="settingIcon" alt="settings" class="icon-img" />
        </button>
      </div>
    </header>

    <main class="container">
      <section class="card">
        <div class="card-header">
          <div>
            <h1 class="word">{{ current.word }}</h1>
            <div class="phonetic">{{ current.phonetic }}</div>
            <div class="pos">{{ current.pos }}</div>
          </div>
          <div class="badge">
            <span class="dot" v-if="current.isNew"></span>
            <small>Новое слово</small>
          </div>
        </div>

        <div class="reveal-area">
          <button class="reveal-btn" @click="show = !show">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none"><path d="M12 5c-7 0-11 7-11 7s4 7 11 7 11-7 11-7-4-7-11-7zm0 11a4 4 0 1 1 .001-7.999A4 4 0 0 1 12 16z" fill="currentColor" /></svg>
            <span>{{ show ? 'Скрыть' : 'Показать' }}</span>
          </button>

          <div class="meaning" v-if="show">
            <p class="translation">{{ current.translation }}</p>
            <p class="example" v-if="current.example">• {{ current.example }}</p>
          </div>
        </div>

        <div class="spacer"></div>

        <div class="progress">
          <div class="progress-pill">{{ index + 1 }} / {{ words.length }}</div>
        </div>
      </section>
    </main>

    <footer class="actions">
      <button class="btn known" @click="markKnown">Я уже знаю это слово</button>
      <button class="btn unknown" @click="markUnknown">Я не знаю этого слова</button>
    </footer>
  </div>
</template>

<script setup>
  import './styles.50cbd9fa588c4d28.css'
  import { ref, computed } from 'vue'

  // Импортируйте реальные файлы. Путь: src/components -> ../assets/...
  // Если у вас другой путь или расширение (svg/png/jpg) — поправьте здесь.
  import settingIcon from '../assets/setting_3405855.png'
  import barIcon from '../assets/bar_16214388.png'

  const words = ref([
    { word: 'Card', phonetic: '[kɑːrd]', pos: 'Noun', translation: 'карточка, карта', example: 'He showed me his card.', isNew: true },
    { word: 'Apple', phonetic: '[ˈæpəl]', pos: 'Noun', translation: 'яблоко', example: 'I ate an apple.', isNew: false },
    // добавьте другие слова по необходимости
  ])

  const index = ref(0)
  const show = ref(false)

  const current = computed(() => words.value[index.value])

  function markKnown() {
    next()
  }

  function markUnknown() {
    next()
  }

  function next() {
    show.value = false
    if (index.value < words.value.length - 1) index.value++
    else index.value = 0
  }

  function onStats() {
    console.log('Open stats')
  }

  function onSettings() {
    console.log('Open settings')
  }

  function markСomplexity() {
    console.log('Change complexity')
  }
</script>

<style scoped>
  .page {
    position: fixed;
    inset: 0;
    display: flex;
    flex-direction: column;
    background: #f6f7fb;
    font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    color: #111;
    padding: 18px;
    box-sizing: border-box;
    overflow: auto;
  }

  /* иконки */
  .icons {
    display: flex;
    gap: 8px;
  }

  .icon-btn {
    background: #fff;
    border: none;
    padding: 10px;
    border-radius: 12px;
    box-shadow: 0 6px 12px rgba(16,24,40,0.06);
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    justify-content: center;
  }

  .icon-img {
    width: 20px;
    height: 20px;
    object-fit: contain;
    display: block;
  }

  /* сохраняем существующий topbar */
  .topbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
    margin-bottom: 14px;
  }

  /* контейнер — центрирует карточку, оставляет запас снизу под фиксированные кнопки */
  .container {
    flex: 1 1 auto;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    padding-bottom: 110px; /* чтобы содержимое не перекрывал footer */
    box-sizing: border-box;
  }

  /* карточка теперь гибкая: занимает доступную высоту */
  .card {
    width: 100%;
    max-width: 420px;
    background: #fff;
    border-radius: 18px;
    padding: 22px;
    box-shadow: 0 12px 24px rgba(16,24,40,0.06);
    display: flex;
    flex-direction: column;
    flex: 1 1 0;
    min-height: 0; /* важно для корректной прокрутки внутри flex */
    box-sizing: border-box;
    overflow: auto;
  }

  /* остальные стили */
  .word {
    font-size: 48px;
    margin: 0;
    line-height: 1;
    font-weight: 700;
  }

  .phonetic {
    color: #6b7280;
    margin-top: 6px;
    font-size: 16px;
  }

  .pos {
    color: #9ca3af;
    font-size: 14px;
    margin-top: 4px;
  }

  .badge {
    text-align: right;
    font-size: 12px;
    color: #6b7280;
    display: flex;
    flex-direction: column;
    align-items: flex-end;
    gap: 6px;
  }

  .dot {
    width: 9px;
    height: 9px;
    background: #0ea5e9;
    border-radius: 50%;
  }

  .reveal-area {
    margin-top: 26px;
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .reveal-btn {
    width: 80%;
    max-width: 320px;
    background: #fff;
    border: 1px solid #e6e6e9;
    padding: 12px 14px;
    border-radius: 10px;
    display: inline-flex;
    gap: 10px;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    box-shadow: 0 6px 12px rgba(16,24,40,0.04);
  }

    .reveal-btn svg {
      opacity: 0.9
    }

  .meaning {
    margin-top: 18px;
    width: 86%;
    color: #374151;
  }

  .translation {
    font-weight: 600;
  }

  .example {
    color: #6b7280;
    margin-top: 8px;
  }

  .spacer {
    flex: 1
  }

  .progress {
    display: flex;
    justify-content: center;
    margin-top: 8px;
  }

  .progress-pill {
    background: rgba(0,0,0,0.7);
    color: #fff;
    padding: 6px 12px;
    border-radius: 8px;
    font-weight: 600;
    font-size: 14px;
  }

  /* фиксируем нижние кнопки по низу окна */
  .actions {
    position: absolute;
    left: 18px;
    right: 18px;
    bottom: 18px;
    display: flex;
    gap: 12px;
    justify-content: center;
    z-index: 30;
  }

  .btn {
    flex: 1;
    max-width: 420px;
    padding: 14px 16px;
    border-radius: 12px;
    border: none;
    cursor: pointer;
    font-weight: 600;
    box-shadow: 0 8px 18px rgba(16,24,40,0.06);
  }

  .known {
    background: #ffdd2d;
    color: #111;
  }

  .unknown {
    background: #111;
    color: #fff;
    border: 1px solid #e6e6e9;
  }

  .complexity {
    background: #ffdd2d;
    color: #111;
    border: 1px solid #e6e6e9;
  }
</style>
