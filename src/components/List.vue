<template>
    <ul class="list-group">
        <Listing v-for="item of items" :key="item.address" :item="item"/>
    </ul>
</template>

<script>
    import Listing from "./Listing";

    export default {
        props: {
            items: Array
        },
        components: {
            Listing
        }
    };
</script>