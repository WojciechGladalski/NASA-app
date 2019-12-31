<template>
    <div id="mainDiv">
        <b-card-body>


            <b-card-text id="title">
                Weather on Mars
            </b-card-text>

            <div id="content" v-show="showContent === true">

                <div id="dataTable">
                    <b-table-simple hover small caption-top responsive>
                        <caption>Weather data:</caption>
                        <b-thead head-variant="dark">
                            <b-tr>
                                <b-th colspan="1" rowspan="2" class="align-middle">SOL</b-th>
                                <b-th colspan="3">T (°C)</b-th>
                                <b-th colspan="3">Pressure (Pa)</b-th>
                                <b-th colspan="1" rowspan="2" class="align-middle">First datum from sensor</b-th>
                                <b-th colspan="1" rowspan="2" class="align-middle">Last datum from sensor</b-th>
                            </b-tr>
                            <b-tr>
                                <b-th>MIN</b-th>
                                <b-th>AVG</b-th>
                                <b-th>MAX</b-th>
                                <b-th>MIN</b-th>
                                <b-th>AVG</b-th>
                                <b-th>MAX</b-th>
                            </b-tr>
                        </b-thead>
                        <b-tbody>
                            <b-tr
                                    v-for="(sol, index) in weather.sol_keys"
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
            toggleContent: function() {
                if (!this.showContent) {
                    this.showContent = true
                } else {
                    this.showContent = false
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

</style>