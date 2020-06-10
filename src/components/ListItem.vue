<template>
    <li @click="$emit('click', $vnode.key)">{{item}}</li>
</template>

<script>
    export default {
        props: ["item"]
    }
</script>