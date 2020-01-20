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
                    <b-img v-bind:src="photo.hdurl" fluid-grow alt="Fluid-grow image" v-if="photo.media_type == 'image'"></b-img>
                    <div class="model-box" v-else-if="photo.media_type == 'video'">
                        <iframe class="model" v-bind:src="photo.url"></iframe>
                    </div>
                    <div class="text-danger" v-else>Unknown media type in database!</div>
                </b-card-body>
                <b-card-text class="text-justify col-12">
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
                immediate: true,
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
        font-size: 1.95em;
    }
    #mainDiv {
        margin-top: 1rem;
        margin-bottom: 1.1rem;
    }

    .model-box {
        position:   relative;
        width:      70%;
        height: 0;
        padding-top:   46.62%;
    }

    .model {
        position: absolute;
        top:      0;
        left:     0;
        bottom:   0;
        right:    0;
        width:    100%;
        height:   100%
    }

</style>