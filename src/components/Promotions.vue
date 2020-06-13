<template>
    <div>
        <div class="row fav" v-for="(promotion, idx) in promotions" :key="idx">

            <div class="col-1-of-2">
                <img alt="Vue logo" :src="promotion.promotion_image">
            </div>

            <div class="col-2-of-2">
                <p class="promo" v-html="promotion.promo_text"></p>
            </div>

        </div>
    </div>
</template>

<script>
    export default {
        name: "Promotions",

        props: {
            promotions: {
                type: Array
            }
        }
    }
</script>

<style scoped>
    img {
        width: 40%;
        height: auto;
        align-content: center;
    }
</style>
