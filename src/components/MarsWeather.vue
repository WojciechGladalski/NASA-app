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
                                <b-th colspan="1" rowspan="2" class="align-middle">Season</b-th>
                                <b-th colspan="1" rowspan="2" class="align-middle col-2">First datum from sensor</b-th>
                                <b-th colspan="1" rowspan="2" class="align-middle col-2">Last datum from sensor</b-th>
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
                                <b-td>{{weather[sol].PRE.mn}}</b-td>
                                <b-td>{{weather[sol].PRE.av}}</b-td>
                                <b-td>{{weather[sol].PRE.mx}}</b-td>
                                <b-td>{{weather[sol].Season}}</b-td>
                                <b-td>{{weather[sol].First_UTC}}</b-td>
                                <b-td>{{weather[sol].Last_UTC}}</b-td>
                            </b-tr>
                        </b-tbody>
                    </b-table-simple>
                </div>

                <div id="chart-demo">
                    <DxPolarChart
                            v-if="showContent === true"
                            id="radarChart"
                            :data-source="periodValues"
                            palette="Soft"
                            title="Wind Rose (samples quantity)"
                            @legend-click="onLegendClick"
                    >
                        <DxCommonSeriesSettings type="stackedbar"/>
                        <DxSeries
                                v-for="wind in windSources"
                                :key="wind.value"
                                :value-field="wind.value"
                                :name="wind.name"
                        />
                        <DxMargin
                                :bottom="50"
                                :left="100"
                        />
                        <DxArgumentAxis
                                :first-point-on-start-angle="true"
                                discrete-axis-division-mode="crossLabels"
                        />
                        <DxValueAxis :value-margins-enabled="false"/>
                        <DxExport :enabled="true"/>
                    </DxPolarChart>
                    <div class="center">

                    </div>
                    <div class="center">
                        <DxSelectBox
                                v-if="showContent === true"
                                :width="110"
                                :data-source="windRoseData"
                                :value.sync="periodValues"
                                display-expr="period"
                                value-expr="values"
                        />
                    </div>
                </div>

            </div>

        </b-card-body>

        <b-button variant="info" v-if="showContent === false" @click="toggleContent">Show</b-button>
        <b-button variant="info" v-else-if="showContent === true" @click="toggleContent">Hide</b-button>

    </div>
</template>

<script>
    import DxSelectBox from 'devextreme-vue/select-box';
    import {windSources} from '../windScopeTemplate.js';
    import {
        DxPolarChart,
        DxCommonSeriesSettings,
        DxSeries,
        DxArgumentAxis,
        DxValueAxis,
        DxMargin,
        DxExport
    } from 'devextreme-vue/polar-chart';

    export default {
        name: "MarsWeather",
        props: {
            weather: Object
        },
        components: {
            DxSelectBox,
            DxPolarChart,
            DxCommonSeriesSettings,
            DxSeries,
            DxArgumentAxis,
            DxValueAxis,
            DxMargin,
            DxExport
        },
        data() {
            return {
                showContent: false,
                windSources,
                windRoseData: [],
                periodValues: []
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
            parseWeatherDates: function () {
                this.weather.sol_keys.forEach((key) => {
                    let firstUTCsplitted = this.weather[key].First_UTC.split('T')
                    let dateFromFirstUTCsplitted = firstUTCsplitted[0].split('-')
                    this.weather[key].First_UTC = dateFromFirstUTCsplitted[2] + '-' + dateFromFirstUTCsplitted[1] + '-' + dateFromFirstUTCsplitted[0] + ', ' + firstUTCsplitted[1].substring(0, 5)

                    let lastUTCsplitted = this.weather[key].Last_UTC.split('T')
                    let dateFromLastUTCsplitted = lastUTCsplitted[0].split('-')
                    this.weather[key].Last_UTC = dateFromLastUTCsplitted[2] + '-' + dateFromLastUTCsplitted[1] + '-' + dateFromLastUTCsplitted[0] + ', ' + lastUTCsplitted[1].substring(0, 5)
                })
            },
            parseWindSamplesData: function () {
                this.weather.sol_keys.forEach((key) => {
                    let period = 'SOL ' + key
                    let values = []

                    let directions = ['N', 'NNE', 'NE', 'ENE', 'E', 'ESE', 'SE', 'SSE', 'S', 'SSW', 'SW', 'WSW', 'W', 'WNW', 'NW', 'NNW']

                    for (let i = 0; i <= 15; i++) {

                        let val1 = 0
                        let val2 = 0
                        let val3 = 0
                        let val4 = 0
                        let val5 = 0
                        let val6 = 0

                        if (this.weather[key].WD[i] !== undefined) {

                            if (this.weather[key].WD[i].ct <= 5000) {
                                val1 = this.weather[key].WD[i].ct
                            } else if (this.weather[key].WD[i].ct <= 10000) {
                                val1 = 5000
                                val2 = this.weather[key].WD[i].ct - 5000
                            } else if (this.weather[key].WD[i].ct <= 15000) {
                                val1 = 5000
                                val2 = 5000
                                val3 = this.weather[key].WD[i].ct - 10000
                            } else if (this.weather[key].WD[i].ct <= 20000) {
                                val1 = 5000
                                val2 = 5000
                                val3 = 5000
                                val4 = this.weather[key].WD[i].ct - 15000
                            } else if (this.weather[key].WD[i].ct <= 25000) {
                                val1 = 5000
                                val2 = 5000
                                val3 = 5000
                                val4 = 5000
                                val5 = this.weather[key].WD[i].ct - 20000
                            } else if (this.weather[key].WD[i].ct <= 30000) {
                                val1 = 5000
                                val2 = 5000
                                val3 = 5000
                                val4 = 5000
                                val5 = 5000
                                val6 = this.weather[key].WD[i].ct - 25000
                            }
                        }

                        values.push({
                            arg: directions[i],
                            val1: val1,
                            val2: val2,
                            val3: val3,
                            val4: val4,
                            val5: val5,
                            val6: val6
                        })

                    }
                    this.windRoseData.push({period, values})
                })
                this.windRoseData.reverse()
                this.periodValues = this.windRoseData[0].values
            },
            onLegendClick({target: series}) {
                if (series.isVisible()) {
                    series.hide();
                } else {
                    series.show();
                }
            }
        },
        watch: {
            weather: {
                immediate: true,
                handler() {
                    this.parseWeatherDates()
                    this.parseWindSamplesData()
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
        margin-bottom: 2.5rem;
    }

    #dataTable {
        margin-bottom: 2.5rem;
    }

    #radarChart {
        height: 500px;
    }

    #chart-demo > .center {
        text-align: center;
    }

    #chart-demo > .center > .dx-widget {
        display: inline-block;
        vertical-align: middle;
    }

    #windTitle {
        font-size: 1.3em;
    }

</style>