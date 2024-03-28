<template>
    <div className="wrapper">
        <div className="heading">
            <span className="material-symbols-outlined">
                thermostat
            </span>
            <h1>Weather</h1>
        </div>
        <p className="error">{{ error }}</p>
        <div className="intersection">
            <span class="material-symbols-outlined">
                search
            </span>
            <input type="text" placeholder="Your location?" v-model="city">
            <button v-show="city != ''" type="button" @click="getWeather()">Find</button>
        </div>
        <div className="location">
            <p>{{ city == "" ? "City is not pointed out" : cityName }}</p>
            <p v-show="info != null">{{ info }} ℃</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            city: "",
            error: "",
            info: null
        }
    },
    computed: {
        cityName() {
            return "Your location: «" + this.city + "»"
        }
    },
    methods: {
        getWeather() {
            if (this.city.trim().length < 3) {
                this.error = "More than 2 symbols!"
                return false
            }
            this.error = ""

            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3d9de74844d28377e81415151cbe6a66`)
                .then(res => (this.info = res.data.main.temp))
        }
    }
}
</script>

<style scoped>
.wrapper {
    width: 900px;
    height: 500px;
    border-radius: 1rem;
    background-color: aliceblue;
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 2rem;
    align-items: center;
    padding: 5% 10%;
}

.heading {
    display: flex;
    flex-direction: row;
    align-items: center;
    font-size: 2rem;
    user-select: none;
}

.heading h1 {
    font-weight: 400;
}

.heading span {
    font-size: 3.5rem;
}

.intersection {
    position: relative;
    display: flex;
    flex-direction: row;
    width: 100%;
    gap: 2rem;
}

.wrapper .intersection input {
    width: 100%;
    height: 3.5rem;
    border-radius: 1rem;
    padding: 0 5%;
    font-size: 2rem;
    border-color: rgb(46, 46, 46);
    transition: all ease 0.3s;
}

.wrapper .intersection input:focus {
    border-color: rgb(0, 0, 0);
}

.intersection span {
    position: absolute;
    left: 0;
    top: 0;
    transform: translateY(35%) translateX(30%);
}

.intersection button {
    width: 120px;
    border-radius: 1rem;
    font-size: 2rem;
    cursor: pointer;
    transition: all ease 0.3s;
    border-color: rgb(46, 46, 46);
}

.intersection button:hover {
    background-color: rgb(233, 233, 233);
    border-color: rgb(0, 0, 0);
}

.error {
    color: brown;
}

.location {
    text-align: center;
}

.location p:last-child {
    font-size: 5rem;
}
</style>