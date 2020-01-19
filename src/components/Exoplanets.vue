<template>
    <div id="mainDiv">
        <b-card-body>


            <b-card-text id="title">
                Exoplanets
            </b-card-text>

            <div>

            </div>

        </b-card-body>

        <b-button variant="info" v-if="0 === 0">Coming Soon!</b-button>
        <b-button variant="info" v-else-if="0 === 1">null</b-button>

    </div>
</template>

<script>
    export default {
        name: "Exoplanets"
    }
</script>

<style scoped>
    #title {
        font-size: 1.95em;
    }
    #mainDiv {
        margin-bottom: 2.1rem;
    }

</style>