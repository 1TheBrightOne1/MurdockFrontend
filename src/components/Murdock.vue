<template>
    <div class="container">
        <div class="row">
           <ul class="nav nav-tabs">
                <li v-for="tab of tabs" :key="tab" class="nav-item"><a class="nav-link">{{tab}}</a></li>
            </ul>
        </div>
        <div class="row">
            <div class="container col" id="propertyFilters">
                <div class="row mb-2">
                    <div class="col-xs-12 col-md-3">
                        <div id="current-distance">Within: {{maxDistance}} miles</div>
                        <input id="distance-slider" style="width:100%;" v-model="maxDistance" type="range" min=".1" max="5" value=".2" step=".1" class="slider"/>
                    </div>
                    <div class="col-sm-2 col-md-3">
                        <div>Min Price</div>
                        <input type="number" v-model="minPrice" step="1"/>
                    </div>
                    <div class="col-sm-2 col-md-3">
                        <div>Max Price</div>
                        <input type="number" v-model="maxPrice" step="1"/>
                    </div>
                </div>
                <div class="row">
                    <List class="col-md-6" :items="property_filter"/>
                    <div class="col-md-6">
                        <Map v-if="properties.length > 0" :markers="property_filter" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import List from "./List";
    import Map from "./Map";
    export default {
        data() {
            return {
                properties: [],
                maxDistance: .25,
                minPrice: 0,
                maxPrice: 500000,
                tabs: ["one", "two"]
            };
        },
        components: {
            List,
            Map
        },
        mounted: function() {
            fetch("http://192.168.1.138:3000/api/murdockhouses").then(result=>result.json()).then(result=>{
                this.properties = result;
            }).catch(err=>{
                alert(err);
            });
        },
        computed: {
            property_filter: function() {
                let filtered_properties = [];
                for(let property of this.properties) {
                    let price = Number.parseInt(property.price.replace(/[$,]/g, ""));
                    if (property.trailDistance < this.maxDistance && 
                        price >= this.minPrice &&
                        price <= this.maxPrice) {
                        filtered_properties.push(property);
                    }
                }
                return filtered_properties;
            }
        }
    }
</script>

<!--<style>
    .container{
        display: flex;
    }

    .map{
        width: 500px;
    }
</style>-->