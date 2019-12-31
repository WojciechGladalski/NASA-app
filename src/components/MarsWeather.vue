<template>
    <div id="mainDiv">
        <b-card-body>


            <b-card-text id="title">
                Weather on Mars
            </b-card-text>

            <div id="content" v-show="showContent === true">

                <div align="center" id="description">
                    <b-card-text class="col-8 text-justify">
                        NASA’s InSight Mars lander takes continuous weather measurements (temperature, wind, pressure)
                        on the surface of Mars at Elysium Planitia, a flat, smooth plain near Mars’ equator.
                    </b-card-text>
                </div>

                <div id="dataTable" class="col-12">
                    <b-table-simple hover small caption-top responsive>
                        <b-thead head-variant="dark">
                            <b-tr>
                                <b-th colspan="1" rowspan="2" class="align-middle col-1">SOL</b-th>
                                <b-th colspan="3" class="col-3">Temperature (°C)</b-th>
                                <b-th colspan="3" class="col-3">Pressure (Pa)</b-th>
                                <b-th colspan="1" rowspan="2" class="align-middle">First datum from sensor</b-th>
                                <b-th colspan="1" rowspan="2" class="align-middle">Last datum from sensor</b-th>
                            </b-tr>
                            <b-tr>
                                <b-th>Min</b-th>
                                <b-th>Avg</b-th>
                                <b-th>Max</b-th>
                                <b-th>Min</b-th>
                                <b-th>Avg</b-th>
                                <b-th>Max</b-th>
                            </b-tr>
                        </b-thead>
                        <b-tbody>
                            <b-tr
                                    v-for="(sol, index) in weather.sol_keys.slice().reverse()"
                                    v-bind:key="index"
                                    class="text-light"
                            >
                                <b-td>{{sol}}</b-td>
                                <b-td>{{weather[sol].AT.mn}}</b-td>
                                <b-td>{{weather[sol].AT.av}}</b-td>
                                <b-td>{{weather[sol].AT.mx}}</b-td>
                                <b-td>{{weather[sol].AT.mn}}</b-td>
                                <b-td>{{weather[sol].AT.av}}</b-td>
                                <b-td>{{weather[sol].AT.mx}}</b-td>
                                <b-td>{{weather[sol].First_UTC}}</b-td>
                                <b-td>{{weather[sol].Last_UTC}}</b-td>
                            </b-tr>
                        </b-tbody>
                    </b-table-simple>
                </div>

            </div>


        </b-card-body>

        <b-button variant="info" v-if="showContent === false" @click="toggleContent">Show</b-button>
        <b-button variant="info" v-else-if="showContent === true" @click="toggleContent">Hide</b-button>

    </div>
</template>

<script>
    export default {
        name: "MarsWeather",
        props: {
            weather: Object
        },
        data() {
            return {
                showContent: false
            }
        },
        methods: {
            toggleContent: function () {
                if (!this.showContent) {
                    this.showContent = true
                } else {
                    this.showContent = false
                }
            },
            // parseWeatherDates: function() {
            //     this.weather.sol_keys.forEach((key) => {
            //         let firUTC = this.weather[key].First_UTC
            //         let lasUTC = this.weather[key].Last_UTC
            //
            //         let f2 = firUTC.split('T')
            //
            //
            //         this.weather[key].First_UTC =
            //         this.weather[key].Last_UTC =
            //     })
            // }
        },
        watch: {
            weather: {
                immediate: true,
                handler() {
                    //this.parseWeatherDates()
                }
            }
        }
    }
</script>

<style scoped>
    #title {
        font-size: 1.5em;
    }

    #mainDiv {
        margin-bottom: 1.1rem;
    }
    #description {
        margin-bottom: 1.25rem;
    }

</style>