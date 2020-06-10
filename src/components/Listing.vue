<template>
    <li class="list-group-item container">
        <div class="row">
            <div class="col-xs-12 col-sm-4 text-center">
                <img class="image" :src="image_address"/>
            </div>
            <span class="property_info col col-sm-4 container justify-content-center">
                <span class="address row pl-3"><a :href="url" target="blank">{{item.address}}</a></span>
                <span class="price row pl-3">{{item.price}}</span>
            </span>
            <span class="distance col col-sm-2">{{item.trailDistance.toFixed(2)}} miles</span>
        </div>
    </li>
</template>

<script>
    export default {
        props: ["item"],
        computed: {
            image_address: function() {
                return encodeURI("http://192.168.1.138:3000/" + this.item.address + ".jpg");
            },

            url: function() {
                return "http://www.aspencreekrealty.com" + this.item.url;
            }
        }
    }
</script>

<!--<style>
li.listing {
    border: 1px solid rgb(120, 120, 120);
    margin: 1%;
    padding: 1%;
    list-style: none;
}

span.property_info {
    display: inline-block;
    width: 40%;
    vertical-align: top;
    padding-left: 1%;
}

span.property_info span {
    display: block;
    width: 50%;
}

span.price {
    font-size: 24px;
    font-weight: bold;
}

span.distance {
    float: right;
}
</style>-->