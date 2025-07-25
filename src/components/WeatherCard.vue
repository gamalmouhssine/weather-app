<template>
    <div v-if="loading" class="weather-card loading">
      <div class="loading-spinner"></div>
      <p>Loading weather data...</p>
    </div>
    
    <div v-else-if="error" class="error-message">
      {{ error }}
    </div>
    
    <div v-else-if="weather" class="weather-card">
      <h2 class="weather-location">
        {{ weather.name }}, {{ weather.sys.country }}
      </h2>
      
      <img
        :src="`https://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`"
        :alt="weather.weather[0].description"
        class="weather-icon"
      />
      
      <div class="weather-temp">
        {{ Math.round(weather.main.temp) }}°C
      </div>
      
      <p class="weather-description">
        {{ weather.weather[0].description }}
      </p>
      
      <div class="weather-details">
        <div class="weather-detail">
          <div class="weather-detail-label">Feels like</div>
          <div class="weather-detail-value">{{ Math.round(weather.main.feels_like) }}°C</div>
        </div>
        
        <div class="weather-detail">
          <div class="weather-detail-label">Humidity</div>
          <div class="weather-detail-value">{{ weather.main.humidity }}%</div>
        </div>
        
        <div class="weather-detail">
          <div class="weather-detail-label">Wind Speed</div>
          <div class="weather-detail-value">{{ Math.round(weather.wind.speed * 3.6) }} km/h</div>
        </div>
        
        <div class="weather-detail">
          <div class="weather-detail-label">Pressure</div>
          <div class="weather-detail-value">{{ weather.main.pressure }} hPa</div>
        </div>
      </div>
    </div>
    
    <div v-else class="empty-state">
      <div class="empty-state-icon">🔍</div>
      <p>Search for a city to see the weather</p>
    </div>
  </template>
  
  <script setup>
  defineProps({
    weather: {
      type: Object,
      default: null
    },
    loading: {
      type: Boolean,
      default: false
    },
    error: {
      type: String,
      default: null
    }
  });
  </script>

<style scoped>
.weather-card {
  background: var(--glass);
  border-radius: var(--border-radius);
  box-shadow: var(--shadow);
  border: 2px solid transparent;
  background-clip: padding-box;
  position: relative;
  padding: 2.5rem 2rem 2rem 2rem;
  margin: 1.5rem 0;
  color: var(--text-color);
  overflow: hidden;
  z-index: 1;
}
.weather-card::before {
  content: '';
  position: absolute;
  inset: -3px;
  z-index: -1;
  border-radius: calc(var(--border-radius) + 4px);
  background: linear-gradient(120deg, var(--primary-color), var(--secondary-color), var(--accent-color), #ce93d8, #f06292, #9575cd);
  background-size: 300% 300%;
  animation: border-animate 6s ease-in-out infinite;
  filter: blur(2px) brightness(1.2);
}
@keyframes border-animate {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}
.weather-location {
  font-size: 1.6rem;
  font-weight: 700;
  letter-spacing: 1px;
  color: #ffe6fa;
  text-shadow: 0 2px 8px var(--primary-color), 0 0 2px var(--secondary-color);
  margin-bottom: 0.5rem;
}
.weather-icon {
  width: 110px;
  height: 110px;
  margin: 1rem 0 0.5rem 0;
  filter: drop-shadow(0 0 16px var(--primary-color)) drop-shadow(0 0 8px var(--secondary-color));
  background: rgba(255,255,255,0.1);
  border-radius: 50%;
}
.weather-temp {
  font-size: 3.2rem;
  font-weight: 800;
  background: linear-gradient(90deg, var(--secondary-color), var(--primary-color), var(--accent-color));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  text-fill-color: transparent;
  margin: 0.5rem 0 0.2rem 0;
  text-shadow: 0 2px 12px var(--primary-color), 0 0 2px var(--accent-color);
}
.weather-description {
  font-size: 1.1rem;
  color: #f8bbd0;
  margin: 0.5rem 0 1.2rem 0;
  text-transform: capitalize;
  letter-spacing: 0.5px;
  text-shadow: 0 1px 4px var(--primary-color);
}
.weather-details {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.2rem;
  margin-top: 1.2rem;
}
.weather-detail {
  background: rgba(255, 255, 255, 0.10);
  border-radius: 16px;
  padding: 1.1rem 0.7rem;
  text-align: center;
  box-shadow: 0 2px 8px 0 var(--primary-color)22, 0 0 0 2px var(--accent-color)15;
  border: 1.5px solid var(--primary-color)33;
  color: var(--text-color);
  transition: background 0.3s;
}
.weather-detail-label {
  font-size: 0.85rem;
  color: #ce93d8;
  text-transform: uppercase;
  letter-spacing: 0.7px;
  margin-bottom: 0.2rem;
}
.weather-detail-value {
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--text-color);
  text-shadow: 0 1px 6px var(--accent-color);
}
.loading-spinner {
  border-top-color: var(--secondary-color);
  border-right-color: var(--primary-color);
  border-bottom-color: var(--accent-color);
  border-left-color: #ce93d8;
}
.error-message {
  background: linear-gradient(90deg, var(--secondary-color) 0%, var(--primary-color) 100%);
  color: var(--text-color);
  border-radius: 18px;
  border-left: 4px solid #f06292;
  box-shadow: 0 2px 8px var(--primary-color)44;
}
.empty-state-icon {
  font-size: 3rem;
  color: var(--primary-color);
  text-shadow: 0 2px 8px var(--secondary-color);
}
</style>