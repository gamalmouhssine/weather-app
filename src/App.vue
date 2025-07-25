<template>
  <div class="app">
    <div class="starry-twinkle">
      <span v-for="n in 30" :key="n" :style="starStyle(n)"></span>
    </div>
    <header class="app-header">
      <h1 class="app-title">
        <span class="star-icon">★</span>
        Weather App
        <span class="star-icon">★</span>
      </h1>
    </header>
    
    <main class="app-main">
      <SearchBar @search="getWeather" :loading="loading" />
      <WeatherCard 
        :weather="weather" 
        :loading="loading" 
        :error="error" 
      />
    </main>
    
    <footer class="app-footer">
      <p>Made with ❤️ by 
        <a 
          href="https://github.com/gamalmouhssine" 
          target="_blank" 
          rel="noopener noreferrer"
          class="author-link"
        >
          Gamal Mouhssine
        </a>
      </p>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';
import SearchBar from './components/SearchBar.vue';
import WeatherCard from './components/WeatherCard.vue';

const weather = ref(null);
const loading = ref(false);
const error = ref(null);

const starColors = [
  '#fff8dc', '#bfa14a', '#fff', '#6ec6ff', '#ffb6c1', '#b39ddb', '#00e5c3'
];
function starStyle(n) {
  // Randomize position and color for each star
  const left = Math.random() * 100;
  const top = Math.random() * 100;
  const size = 1 + Math.random() * 2;
  const color = starColors[n % starColors.length];
  const delay = (Math.random() * 2).toFixed(2) + 's';
  return {
    left: left + 'vw',
    top: top + 'vh',
    width: size + 'px',
    height: size + 'px',
    background: color,
    animationDelay: delay
  };
}

const getWeather = async (city) => {
  try {
    loading.value = true;
    error.value = null;
    
    const apiKey = import.meta.env.VITE_WEATHER_API_KEY;
    
    if (!apiKey) {
      throw new Error('API key not found. Please set VITE_WEATHER_API_KEY in your environment variables.');
    }
    
    const url = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(city)}&appid=${apiKey}&units=metric`;
    const response = await axios.get(url);
    
    weather.value = response.data;
  } catch (err) {
    console.error('Error fetching weather data:', err);
    
    if (err.response?.status === 404) {
      error.value = 'City not found. Please check the spelling and try again.';
    } else if (err.response?.status === 401) {
      error.value = 'Invalid API key. Please check your configuration.';
    } else if (!navigator.onLine) {
      error.value = 'No internet connection. Please check your network.';
    } else {
      error.value = 'Failed to fetch weather data. Please try again later.';
    }
    
    weather.value = null;
  } finally {
    loading.value = false;
  }
};
</script>

<style>
:root {
  --primary-color: #b39ddb; /* purple */
  --secondary-color: #ffb6c1; /* pink */
  --accent-color: #6ec6ff; /* blue */
  --background-color: #f8f6ff;
  --card-background: rgba(60, 0, 90, 0.7);
  --glass: rgba(60, 0, 90, 0.7);
  --text-color: #fff;
  --text-muted: #ce93d8;
  --border-radius: 24px;
  --shadow: 0 8px 32px 0 rgba(80, 0, 120, 0.25), 0 0 0 4px rgba(255,182,193,0.15);
  --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  --gradient-bg: linear-gradient(135deg, #f8f6ff 0%, #ffe6fa 40%, #e3e0ff 100%);
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  padding: 0;
  font-family: 'Georgia', 'Times New Roman', Times, serif;
  background: var(--gradient-bg);
  background-color: var(--background-color);
  min-height: 100vh;
}

.app {
  max-width: 440px;
  margin: 32px auto;
  padding: 32px 20px 24px 20px;
  min-height: 80vh;
  display: flex;
  flex-direction: column;
  border-radius: 32px;
  background: var(--glass);
  box-shadow: var(--shadow);
  position: relative;
  z-index: 2;
  overflow: hidden;
}
.app::before {
  content: '';
  position: absolute;
  inset: -4px;
  z-index: -1;
  border-radius: 36px;
  background: linear-gradient(120deg, #b39ddb, #ffb6c1, #6ec6ff, #ce93d8, #f06292, #9575cd);
  background-size: 300% 300%;
  animation: border-animate 6s ease-in-out infinite;
  filter: blur(3px) brightness(1.2);
}
@keyframes border-animate {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.starry-twinkle {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: -1; 
}

.starry-twinkle span {
  position: absolute;
  background: radial-gradient(circle, rgba(255,255,255,0.8) 0%, rgba(255,255,255,0) 70%);
  border-radius: 50%;
  animation: twinkle 2s infinite ease-in-out alternate;
}

@keyframes twinkle {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

.app-header {
  text-align: center;
  margin-bottom: 2rem;
}
.app-title {
  color: #ffe6fa;
  font-size: 2.7rem;
  font-weight: 800;
  font-family: 'Georgia', 'Times New Roman', Times, serif;
  text-shadow: 0 2px 12px #b39ddb, 0 0 2px #ffb6c1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  background: linear-gradient(90deg, #ffb6c1, #b39ddb, #6ec6ff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
}
.star-icon {
  color: #ffb6c1;
  font-size: 2.2rem;
  margin: 0 0.5rem;
  text-shadow: 0 0 8px #b39ddb, 0 0 2px #ffb6c1;
  vertical-align: middle;
  animation: twinkle 2s infinite ease-in-out alternate;
}
@keyframes twinkle {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

.weather-icon {
  font-size: 2rem;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(-5px);
  }
}

.app-main {
  flex: 1;
}

.app-footer {
  text-align: center;
  margin-top: 2.5rem;
  padding-top: 1.2rem;
  color: #b39ddb;
  font-size: 1rem;
  text-shadow: 0 2px 8px #ffb6c1;
  background: rgba(255,255,255,0.04);
  border-radius: 16px;
}
.app-footer a.author-link {
  color: #6ec6ff;
  text-decoration: none;
  font-weight: 700;
  transition: var(--transition);
  text-shadow: 0 2px 8px #b39ddb;
}
.app-footer a.author-link:hover {
  text-decoration: underline;
  opacity: 0.8;
}

.search-bar {
  margin-bottom: 1.5rem;
  position: relative;
  background: rgba(255,255,255,0.10);
  border-radius: 18px;
  box-shadow: 0 2px 8px 0 rgba(179,157,219,0.10), 0 0 0 2px rgba(110,198,255,0.08);
  border: 1.5px solid rgba(179,157,219,0.18);
  padding: 0.5rem 0.7rem;
}
.search-input {
  width: 100%;
  padding: 16px 20px;
  border: none;
  border-radius: 14px;
  font-size: 16px;
  background-color: rgba(255,255,255,0.18);
  color: #fff;
  transition: var(--transition);
  box-shadow: 0 2px 8px #b39ddb33;
}
.search-input::placeholder {
  color: #ce93d8;
}
.search-input:focus {
  outline: none;
  box-shadow: 0 0 0 3px #b39ddb55, 0 2px 8px #ffb6c133;
  transform: translateY(-2px);
}
.search-input:disabled {
  background-color: #f3e5f5;
  color: #b39ddb;
  cursor: not-allowed;
}

.weather-card {
  background-color: var(--card-background);
  padding: 2rem;
  border-radius: var(--border-radius);
  box-shadow: var(--shadow);
  transition: var(--transition);
  text-align: center;
}

.weather-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-hover);
}

.weather-card.loading {
  opacity: 0.7;
}

.weather-location {
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--text-color);
  margin: 0 0 1rem 0;
}

.weather-temp {
  font-size: 3rem;
  font-weight: 700;
  color: var(--primary-color);
  margin: 0.5rem 0;
}

.weather-description {
  font-size: 1.1rem;
  color: var(--text-muted);
  margin: 0.5rem 0;
  text-transform: capitalize;
}

.weather-details {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-top: 1.5rem;
}

.weather-detail {
  background-color: var(--background-color);
  padding: 1rem;
  border-radius: 12px;
  text-align: center;
}

.weather-detail-label {
  font-size: 0.8rem;
  color: var(--text-muted);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 0.25rem;
}

.weather-detail-value {
  font-size: 1.2rem;
  font-weight: 600;
  color: var(--text-color);
}

.weather-icon {
  width: 120px;
  height: 120px;
  object-fit: contain;
  margin: 1rem 0;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.1));
}

.loading-spinner {
  display: inline-block;
  width: 40px;
  height: 40px;
  border: 4px solid var(--border-color);
  border-radius: 50%;
  border-top-color: var(--primary-color);
  animation: spin 1s ease-in-out infinite;
  margin: 2rem 0;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.error-message {
  background-color: #fff5f5;
  color: var(--error-color);
  padding: 1rem;
  border-radius: var(--border-radius);
  border-left: 4px solid var(--error-color);
  margin: 1rem 0;
  font-weight: 500;
}

.empty-state {
  text-align: center;
  padding: 3rem 1rem;
  color: var(--text-muted);
}

.empty-state-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
  opacity: 0.5;
}

@media (max-width: 480px) {
  .app {
    padding: 16px;
  }
  .app-title {
    font-size: 2rem;
  }
  .weather-card {
    padding: 1.5rem;
  }
  
  .weather-temp {
    font-size: 2.5rem;
  }
  
  .weather-details {
    grid-template-columns: 1fr;
  }
}

@media (prefers-color-scheme: dark) {
  :root {
    --background-color: #1a1a1a;
    --card-background: #2d2d2d;
    --text-color: #ffffff;
    --text-muted: #a0a0a0;
    --border-color: #404040;
  }
}
</style>