<template>
    <div id="mainDiv">
        <b-card-body>

            <b-card-text id="title">
                Weather on Mars
            </b-card-text>

            <div id="content" v-show="showContent === true">

                <div align="center" id="description">
                    <b-card-text class="col-9 text-justify">
                        NASA’s InSight Mars lander takes continuous weather measurements (temperature, wind, pressure)
                        on the surface of Mars at Elysium Planitia, a flat, smooth plain near Mars’ equator.
                    </b-card-text>
                </div>

                <div id="outerTableDiv" align="center">
                    <div id="dataTable" class="col-9 text-center">
                        <b-table-simple hover small caption-top responsive>
                            <b-thead head-variant="dark">
                                <b-tr>
                                    <b-th colspan="1" rowspan="1" class="align-middle">SOL</b-th>
                                    <b-th colspan="1" rowspan="1" class="align-middle">First datum from sensor</b-th>
                                    <b-th colspan="1" rowspan="1" class="align-middle">Last datum from sensor</b-th>
                                    <b-th colspan="1" rowspan="1" class="align-middle">Season</b-th>
                                </b-tr>
                            </b-thead>
                            <b-tbody>
                                <b-tr
                                        v-for="(sol, index) in weather.sol_keys.slice().reverse()"
                                        v-bind:key="index"
                                        class="text-light"
                                >
                                    <b-td>{{sol}}</b-td>
                                    <b-td>{{weather[sol].First_UTC}}</b-td>
                                    <b-td>{{weather[sol].Last_UTC}}</b-td>
                                    <b-td>{{weather[sol].Season.charAt(0).toUpperCase() +
                                        weather[sol].Season.slice(1)}}
                                    </b-td>
                                </b-tr>
                            </b-tbody>
                        </b-table-simple>
                    </div>
                </div>

                <div align="center">
                    <div id="chart-demo-temp" class="col-12">
                        <DxChart
                                id="chart-temp"
                                v-if="showContent === true"
                                :data-source="temperatureData"
                                palette="Violet"
                                title="Temperature (℃) per SOL"
                        >
                            <DxCommonSeriesSettings
                                    :type="type"
                                    argument-field="sol"
                            />
                            <DxCommonAxisSettings>
                                <DxGrid :visible="true"/>
                            </DxCommonAxisSettings>
                            <DxSeries
                                    v-for="tem in temperatureTemplate"
                                    :key="tem.value"
                                    :value-field="tem.value"
                                    :name="tem.name"
                            />
                            <DxMargin :bottom="20"/>
                            <DxArgumentAxis
                                    :allow-decimals="false"
                                    :axis-division-factor="60"
                            >
                                <DxLabel>
                                    <DxFormat type="decimal"/>
                                </DxLabel>
                            </DxArgumentAxis>
                            <DxLegend
                                    vertical-alignment="top"
                                    horizontal-alignment="right"
                            />
                            <DxExport :enabled="true"/>
                            <DxTooltip :enabled="true"/>
                        </DxChart>
                    </div>

                    <div id="chart-demo-press" class="col-12">
                        <DxChart
                                id="chart-press"
                                v-if="showContent === true"
                                :data-source="pressureData"
                                palette="Violet"
                                title="Pressure (Pa) per SOL"
                        >
                            <DxCommonSeriesSettings
                                    :type="type"
                                    argument-field="sol"
                            />
                            <DxCommonAxisSettings>
                                <DxGrid :visible="true"/>
                            </DxCommonAxisSettings>
                            <DxSeries
                                    v-for="press in pressureTemplate"
                                    :key="press.value"
                                    :value-field="press.value"
                                    :name="press.name"
                            />
                            <DxMargin :bottom="20"/>
                            <DxArgumentAxis
                                    :allow-decimals="false"
                                    :axis-division-factor="60"
                            >
                                <DxLabel>
                                    <DxFormat type="decimal"/>
                                </DxLabel>
                            </DxArgumentAxis>
                            <DxLegend
                                    vertical-alignment="top"
                                    horizontal-alignment="right"
                            />
                            <DxExport :enabled="true"/>
                            <DxTooltip :enabled="true"/>
                        </DxChart>
                        <div class=""
                             v-if="showContent === true"
                        >
                            <div class="caption">Options</div>
                            <div class="option">
                                <span>Series Type</span>
                                <DxSelectBox
                                        :data-source="types"
                                        :value.sync="type"
                                />
                            </div>
                        </div>
                    </div>
                </div>

                <h3><strong>Wind Data for chosen SOL</strong></h3>
                <hr id="segregator" class="border-info">

                <div id="chart-demo">
                    <DxPolarChart
                            v-if="showContent === true"
                            id="radarChart"
                            :data-source="periodValues"
                            palette="Soft"
                            title="Quantity of Samples"
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
                                :bottom="17"
                                :left="80"
                        />
                        <DxArgumentAxis
                                :first-point-on-start-angle="true"
                                discrete-axis-division-mode="crossLabels"
                        />
                        <DxValueAxis :value-margins-enabled="false"/>
                        <DxExport :enabled="true"/>
                    </DxPolarChart>

                    <DxChart
                            v-if="showContent === true"
                            id="chart"
                            :data-source="periodValues"
                            title="Wind Speed (m/s)"
                    >
                        <DxSeries
                                argument-field="windSpeed"
                                value-field="windSpeedValue"
                                name=" "
                                type="bar"
                                color="#00C0D0"
                        />
                        <DxMargin
                                :left="57"
                                :bottom="23"
                        />
                        <DxExport :enabled="true"/>
                    </DxChart>


                    <div id="selector" class="center">
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
    import {
        temperatureTemplate,
        pressureTemplate
    } from '../basicWeatherDataTemplate.js';
    import {
        DxChart,
        DxCommonAxisSettings,
        DxGrid,
        DxLegend,
        DxTooltip,
        DxLabel,
        DxFormat
    } from 'devextreme-vue/chart';

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
            DxExport,
            DxChart,
            DxCommonAxisSettings,
            DxGrid,
            DxLegend,
            DxTooltip,
            DxLabel,
            DxFormat
        },
        data() {
            return {
                showContent: false,
                windSources,
                windRoseData: [],
                periodValues: [],
                columns: ['min', 'avg', 'max'],
                types: ['Spline', 'Stackedspline', 'Fullstackedspline'],
                type: 'Spline',
                temperatureData: [],
                temperatureTemplate,
                pressureData: [],
                pressureTemplate
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

                    let thisSolUnableDataCounter = 0

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
                        } else {
                            thisSolUnableDataCounter++
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

                    if (this.weather[key].HWS !== undefined && thisSolUnableDataCounter < 16) {
                        values.push({windSpeed: 'Min', windSpeedValue: this.weather[key].HWS.mn}, {
                            windSpeed: 'Avg',
                            windSpeedValue: this.weather[key].HWS.av
                        }, {windSpeed: 'Max', windSpeedValue: this.weather[key].HWS.mx})

                        //adding to final table
                        this.windRoseData.push({period, values})
                    }
                })
                this.windRoseData.reverse()
                this.periodValues = this.windRoseData[0].values

                // eslint-disable-next-line no-console
                //console.log(this.periodValues)
            },
            onLegendClick({target: series}) {
                if (series.isVisible()) {
                    series.hide();
                } else {
                    series.show();
                }
            },
            parseTemperatureAndPressure: function () {
                this.weather.sol_keys.forEach((key) => {

                    this.temperatureData.push({
                        max: this.weather[key].AT.mx,
                        avg: this.weather[key].AT.av,
                        min: this.weather[key].AT.mn,
                        sol: parseInt(key)
                    })
                    this.pressureData.push({
                        max: this.weather[key].PRE.mx,
                        avg: this.weather[key].PRE.av,
                        min: this.weather[key].PRE.mn,
                        sol: parseInt(key)
                    })
                })
            }
        },
        watch: {
            weather: {
                immediate: true,
                handler() {
                    this.parseWeatherDates()
                    this.parseWindSamplesData()
                    this.parseTemperatureAndPressure()
                }
            }
        }
    }
</script>

<style scoped>
    #title {
        font-size: 1.95em;
    }

    #mainDiv {
        margin-bottom: 1.1rem;
    }

    #description {
        margin-bottom: 4.5rem;
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

    #chart {
        height: 440px;
    }

    #selector {
        margin-bottom: 3.2rem;
        margin-right: 0.31%;
    }

    #segregator {
        width: 45%;
        margin-bottom: 29px;
    }

    .options {
        padding: 20px;
        background-color: rgba(191, 191, 191, 0.15);
        margin-top: 10px;
    }

    .option {
        margin-top: 10px;
    }

    .caption {
        font-size: 18px;
        font-weight: 500;
    }

    .option > span {
        margin-right: 10px;
    }

    .option > .dx-widget {
        display: inline-block;
        vertical-align: middle;
    }

    #outerTableDiv {
        margin-bottom: 3.6rem;
    }

    #chart-demo-press {
        margin-bottom: 4.9rem;
    }

    #chart-demo-temp {
        margin-bottom: 10px;
    }

</style>