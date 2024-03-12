<template>
        <div class="location">
            {{ weatherData.name }}, {{ weatherData.sys.country }}
        </div>

        <div class="weather content">
            <div><i class="bi bi-thermometer-half"> </i>{{ Math.round(weatherData.main.temp) }}&deg;F</div>
            <hr>
            <div><i class="bi bi-droplet"> </i>{{ weatherData.main.humidity }}%</div>
            <hr>
            <div><i class="bi bi-wind"> </i>{{ Math.round(weatherData.wind.speed) }} mph</div>
            <i v-if="weatherData.weather[0].main == 'Clouds'" class="bi bi-cloud-fill"></i>
            <i v-if="weatherData.weather[0].main == 'Rain'" class="bi bi-cloud-rain-fill"></i>

            <i v-if="weatherData.weather[0].main == 'Clear' && weatherData.sys.sunset > weatherData.dt" class="bi bi-sun-fill"></i>
            <i v-if="weatherData.weather[0].main == 'Clear' && weatherData.sys.sunset < weatherData.dt" class="bi bi-moon-fill"></i>
        </div>
</template>

<script>

export default {
    props: ['weatherData'],
}

</script>