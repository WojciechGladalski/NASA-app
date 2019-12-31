<template>
    <div id="mainDiv">
        <b-card-body align="center">

            <b-card-text id="title">
                Photo of The Day
            </b-card-text>
            <div v-show="showContent === true">
                <b-card-text id="titleOfPhoto">
                    {{photo.title}}, {{splittedData}}
                </b-card-text>
                <b-card-body>
                    <b-img v-bind:src="photo.hdurl" fluid-grow alt="Fluid-grow image"></b-img>
                </b-card-body>
                <b-card-text class="text-justify col-11">
                    {{photo.explanation}}
                </b-card-text>
            </div>

        </b-card-body>

        <b-button variant="info" v-if="showContent === false" @click="toggleContent">Show</b-button>
        <b-button variant="info" v-else-if="showContent === true" @click="toggleContent">Hide</b-button>

    </div>
</template>

<script>
    export default {
        name: "PhotoOfTheDay",
        props: {
            photo: Object
        },
        methods: {
            splitData: function() {
                let splittedData = this.photo.date.split('-')
                this.splittedData = splittedData[2] + '.' + splittedData[1] + '.' + splittedData[0]
            },
            toggleContent: function() {
                if (!this.showContent) {
                    this.showContent = true
                } else {
                    this.showContent = false
                }
            }
        },
        data: function () {
            return {
                splittedData: null,
                showContent: false
            }
        },
        watch: {
            photo: {
                immediate: false,
                handler() {
                    this.splitData()
                }
            }
        }
    }
</script>

<style scoped>
    #titleOfPhoto {
        margin-bottom: 0px;
    }
    #title {
        font-size: 1.5em;
    }
    #mainDiv {
        margin-top: 1rem;
        margin-bottom: 1.1rem;
    }
</style>