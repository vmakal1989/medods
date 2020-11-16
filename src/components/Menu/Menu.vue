<template>
    <div class="block">
        <div id="form-image-block" class="image" title="Click to open the form">
            <router-link to="/form" >
                <img src="../../../public/form.jpg" alt="">
            </router-link>
        </div>
        <div id="game-image-block" class="image" title="Click to open the game">
            <router-link to="/simon" >
                <img src="../../../public/game.jpg" alt="" >
            </router-link>
        </div>
    </div>
</template>
<script>
</script>
<style scoped lang="sass">
.block
    margin: 200px auto
    max-width: 520px
    min-height: 260px
    display: flex
    flex-direction: row
    flex-wrap: wrap
.image
    width: 48%
    height: 48%
    margin: 5px
    cursor: pointer
    img
        width: 100%
        height: auto
        display: block
        margin: 0 auto
        border-radius: 15px
        transition: all 0.5s
    img:hover
        transform: scale(1.04)
@media (max-width: 500px)
    .block
        margin: 0 0
        width: 90%
        padding: 20px
        flex-direction: column
    .image
        width: 100%
        height: 100%
        margin: 10px 0 0 0

</style>