<template>
    <div id="mainDiv">
        <b-card-header header-tag="nav">
            <b-nav align="center">
                <b-nav-item disabled id="title">Check interesting things from NASA databases!</b-nav-item>
            </b-nav>
        </b-card-header>
    </div>
</template>

<script>
    export default {
        name: "Header"
    }
</script>

<style scoped>
    #title {
        font-size: 2em;
    }
</style>