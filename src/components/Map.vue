<template>
    <div class="map"/>
</template>

<script>
    export default {
        data() {
            return {
                map: null,
                trailCoords: [],
                googleMarkers: []
            }
        },

        props: {
            markers: Array,
            line_segments: Array
        },

        mounted: function() {
            var script = document.createElement("script");
            script.src = "https://maps.googleapis.com/maps/api/js?key=AIzaSyDdiZaNaz3LxuOkP9CGQy5Z4WPklCuLLuA&callback=googleLoaded";
            script.defer = true;
            script.async = true;

            window.googleLoaded = () => {
                this.map = new google.maps.Map(document.getElementsByClassName('map')[0], {
                    center: {lat: 40.362090, lng: -111.717800},
                    zoom: 11
                });
                this.loadTrailMarkers();
                this.redrawTrailMarkers();
            }

            document.head.appendChild(script);
        },

        methods: {
            loadTrailMarkers: function(){
                fetch("http://192.168.1.138:3000/api/trailmarkers").then(result=>result.json()).then(json=>{
                    /*global google*/
                    this.trailCoords = new google.maps.Polyline({
                        path: json,
                        geodesic: true,
                        strokeColor: "#FF0000",
                        strokeOpacity: 1.0,
                        strokeWeight: 2
                    });

                    this.trailCoords.setMap(this.map);
                });
            },
            redrawTrailMarkers(){
                for (let house of this.markers) {
                    this.googleMarkers.push(new google.maps.Marker({
                        position: new google.maps.LatLng(house.latLng.lat, house.latLng.lng),
                        title: "hello",
                        map: this.map
                    }));
                }
            }
        },
        watch: {
            markers: function(){
                for (let marker of this.googleMarkers){
                    marker.setMap(null);
                }
                this.googleMarkers = [];
                this.redrawTrailMarkers();
            }
        }
    }
</script>

<style>
    .map {
        width: 100%;
        height: 500px;
    }
</style>