<template>
    <div id="main-burger">
        <h1>Make Your Burguer</h1>
    </div>
</template>

<script>
export default {
    name: "Banner"
}
</script>

<style scoped>
    #main-burger{
        background-image: url('/img/burger.jpg');
        background-position: 0 -250px;
        background-size: cover;
        height: 500px;
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }

    #main-burger h1{
        color: #FFF;
        text-align: center;
        font-size: 60px;
        background-color: #222;
        padding: 20px 40px;
    }
</style>