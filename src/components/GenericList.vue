<template>
    <ol class="list-group">
        <ListItem v-for="item of itemContains" :item="item.value" :key="item.key" @click="itemClicked"/>
    </ol>
</template>

<script>
    import ListItem from "./ListItem"

    export default {
        data() {
            return {
                activeItem: ""
            };
        },
        props: {
            items: Array,
            contains: String
        },
        methods: {
            itemClicked: function(value) {
                this.activeItem = value;
                this.$emit('click', value);
            }
        },
        computed: {
            itemContains: function() {
                let filteredList = [];
                for (let item of this.items) {
                    if (!this.contains || item.value.toLowerCase().indexOf(this.contains.toLowerCase()) >= 0) {
                        filteredList.push(item);
                    }
                }
                return filteredList;
            }
        },
        components: {
            ListItem
        }
    }
</script>