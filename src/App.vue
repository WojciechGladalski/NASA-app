<template>
    <div id="app">

        <b-container class="bv-example-row col-sm-8">
            <b-card title="Card Title" no-body class="bg-dark text-light border-info">

                <Header/>

                <PhotoOfTheDay
                        v-if="photo.title !== undefined"
                        v-bind:photo="photo"
                />

                <hr class="border-info">

                <MarsWeather
                        v-if="marsWeather.sol_keys !== undefined"
                        v-bind:weather="marsWeather"
                />

                <hr class="border-info">

                <Exoplanets

                />

            </b-card>
        </b-container>

    </div>
</template>

<script>
    import Header from './components/Header.vue'
    import PhotoOfTheDay from './components/PhotoOfTheDay.vue'
    import MarsWeather from './components/MarsWeather.vue'
    import Exoplanets from "./components/Exoplanets"

    export default {
        name: 'app',
        components: {
            Header,
            PhotoOfTheDay,
            MarsWeather,
            Exoplanets
        },
        mounted: function () {
            fetch('https://api.nasa.gov/planetary/apod?api_key=q6ZJMt3SVi49YwTPTb6DlzmGDgYYA2xSGosNFFyF', {
                method: 'get'
            })
                .then(response => response.json())
                .then((jsonPhoto) => {
                    this.photo = jsonPhoto
                })
            fetch('https://api.nasa.gov/insight_weather/?api_key=q6ZJMt3SVi49YwTPTb6DlzmGDgYYA2xSGosNFFyF&feedtype=json&ver=1.0', {
                method: 'get'
            })
                .then(response => response.json())
                .then((jsonWeather) => {
                    this.marsWeather = jsonWeather
                })
        },
        data: function() {
            return {
                photo: {},
                marsWeather: {}
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

</style>
