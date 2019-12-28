<template>
    <div id="app">

        <b-container class="bv-example-row col-sm-8">
            <b-card title="Card Title" no-body>

                <Header/>

                <PhotoOfTheDay
                        v-if="photo !== {}"
                        v-bind:photo="photo"
                />

                <hr>

                <MarsWeather/>
            </b-card>
        </b-container>

    </div>
</template>

<script>
    import Header from './components/Header.vue'
    import PhotoOfTheDay from './components/PhotoOfTheDay.vue'
    import MarsWeather from './components/MarsWeather.vue'

    export default {
        name: 'app',
        components: {
            Header,
            PhotoOfTheDay,
            MarsWeather
        },
        mounted: function () {
            fetch('https://api.nasa.gov/planetary/apod?api_key=q6ZJMt3SVi49YwTPTb6DlzmGDgYYA2xSGosNFFyF', {
                method: 'get'
            })
                .then(response => response.json())
                .then((jsonPhoto) => {
                    this.photo = jsonPhoto
                })
        },
        data: function() {
            return {
                photo: {}
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
