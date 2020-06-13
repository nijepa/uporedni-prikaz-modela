<template>
    <div class="row fav">

        <div class="col-1-of-2">
            <img alt="Vue logo" :src="imgSrc">
        </div>

        <div class="col-1-of-2">
            <ul v-for="(packageSingle, idx) in packages" :key="idx">
                <li v-if="packageSingle.product_category === categoryName"
                    v-html="boldText(packageSingle.long_name, packageSingle.attributes.attribute_value)">
                </li>
            </ul>
        </div>

    </div>
</template>

<script>
    export default {
        name: "PackageTV",

        props: {
            packages: {
                type: Array
            },
            imgSrc: {
                type: String
            },
            categoryName: {
                type: String
            }
        },

        methods: {
            boldText: function(textToBold, partToBold) {
                return textToBold.replace(partToBold, "<strong>" + partToBold + "</strong>")
            }
        }
    }
</script>

<style scoped>
    img {
        width: 20%;
        height: auto;
        align-content: center;
    }
</style>
