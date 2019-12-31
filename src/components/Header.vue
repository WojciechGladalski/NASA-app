<template>
    <div id="mainDiv">
        <b-card-header header-tag="nav" class="border-info">
            <b-nav align="center">
                <b-nav-item disabled id="title"><span class="text-light">Check interesting things from NASA databases!</span></b-nav-item>
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