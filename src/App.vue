<template>
    <main :class="{ 'warm' : this.is_warm }">
        <input 
            type="text" 
            class="search-bar" 
            placeholder="please enter city name..." 
            v-model="query"
            @keypress="fetchWeather"
        />

        <div v-if="typeof this.response.main != 'undefined' && this.response.cod=='200'" class="response-wrapper">
            <WeatherCard :weatherData="this.response" />
        </div>
        <div v-else-if="this.response.cod=='404'" class="error-container">
            <i class="bi bi-exclamation-triangle-fill"></i> <p class="error-msg">City not found...</p>
        </div>
        <div v-else class="response-wrapper">
            <p v-if="!this.query" class="search-prompt-text">Enter a city name above to get the current weather</p>
        </div>

    </main>
</template>

<script>
import WeatherCard from './components/WeatherCard.vue';

export default {
    name: 'App',
    components: {
        WeatherCard
    },
    data() {
        return {
            query: "",
            old_query: "",
            is_warm: false,
            url_base: "https://api.openweathermap.org/data/2.5/weather?q=",
            api_key: "9e5f5eca8129ce1059f803d63959728e",
            response: {}
        }
    },
    methods: {
        fetchWeather(e) {
            if (!this.query == "" && e.key == "Enter" && this.query != this.old_query) {
                this.old_query = this.query
                this.response = {};
                fetch(`${this.url_base}${this.query}&units=imperial&appid=${this.api_key}`)
                    .then(res => res.json())
                    .then(data => this.setWeather(data))
            }
        },
        setWeather(data) {
            this.response = data;
            this.is_warm = this.tempClass();
        },
        tempClass() {
            if (this.response.main) {
                return (this.response.main.temp > 60);
            }
        }
    }
}
</script>

<style>

html {
    padding: 0;
}

body {
    margin: 0;
}

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    padding: 0;
    margin: 0;
}

main {
    height: 100vh;
    background: rgb(2,0,36);
    background: linear-gradient(45deg, rgba(2,0,36,1) 0%, rgba(9,9,121,1) 10%, rgba(0,212,255,0.35056029247636555) 83%); 
}
.warm {
    background: linear-gradient(45deg, rgba(2,0,36,1) 0%, rgb(175, 66, 20) 10%, rgba(255, 221, 0, 0.351) 83%); 
}

.search-bar {
    width: 90%;
    padding: 10px;
    margin: 0 auto;
    margin-top: 2%;
    font-size: 22px;
    background-color: rgba(0,0,0,.3);
    border: none;
}

.search-bar:focus {
    background-color: rgba(255,255,255,0.4);
}

.response-wrapper {
    margin: 0 auto;
    text-align: center;
    display: flex;
    flex-direction: column;
    max-width: 100%;
}

.search-prompt-text {
    font-size: 1.25em;
}

.location {
    margin: 10px auto;
    padding: 15px 0;
    text-align: center;
    background-color: rgba(255,255,255,0.25);
    width: 100%;
    max-width: 100%;
    font-size: 36px;
    box-shadow: 3px 3px rgba(0,0,0,.3);
    animation: fade-in 0.6s;
    animation-fill-mode: forwards;
    opacity: 0;
}

.weather {
    margin: 10px auto;
    padding: 15px 0;
    text-align: center;
    background-color: rgba(255,255,255,0.25);
    width: 100%;
    max-width: 100%;
    font-size: 36px;
    box-shadow: 3px 3px rgba(0,0,0,.3);
    animation: fade-in 0.6s;
    animation-delay: 0.3s;
    animation-fill-mode: forwards;
    opacity: 0;
}

.weather > div {
    display: flex;
    justify-content: space-between;
    padding: 0 8px;
}

.weather > hr {
    width: 80%;
    border: none;
    border-bottom: 2px solid rgba(0,0,0,0.2);
    margin: 15px auto;
}

.error-container {
    margin: 20px auto;
    padding: 15px;
    text-align: center;
    background-color: rgba(226, 139, 139, 0.705);
    max-width: 100%;
    font-size: 20px;
    box-shadow: 3px 3px rgba(0,0,0,.3);
}

.mach {
    font-size: 16px;
    animation: jitter 0.3s;
    animation-iteration-count: 4;
    transition: 1s ease;
}

@keyframes fade-in {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

@keyframes jitter {
    0% {
        transform: translateX(-4px);
    }
    50% {
        transform: translateX(4px);
    }
    100% {
        transform: translateX(-4px);
    }
}

@media screen and (min-width: 400px) {
    .search-bar {
        font-size: 30px;
    }
    .response-wrapper {
        max-width: 65%;
        margin: 15px auto;
    }
    .error-container {
        font-size: 24px;
        min-width: 65%;
    }
}

@media screen and (min-width: 768px) {
    .search-bar {
        font-size: 42px;
    }
    .response-wrapper {
        max-width: 35%;
        margin: 15px auto;
    }
    .error-container {
        max-width: 35%;
        font-size: 36px;
    }
}

</style>
