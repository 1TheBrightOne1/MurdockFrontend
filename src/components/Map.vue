<template>
    <div class="map"/>
</template>

<script>
    export default {
        data() {
            return {
                map: null,
                trailCoords: []
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

            let that = this;
            window.googleLoaded = () => {
                this.map = new google.maps.Map(document.getElementsByClassName('map')[0], {
                    center: {lat: 40.362090, lng: -111.717800},
                    zoom: 11
                });
                this.loadTrailMarkers(that);
            }

            document.head.appendChild(script);
        },

        methods: {
            loadTrailMarkers: (that)=>{
                fetch("http://192.168.1.138:3000/api/trailmarkers").then(result=>result.json()).then(json=>{
                    /*global google*/
                    that.trailCoords = new google.maps.Polyline({
                        path: json,
                        geodesic: true,
                        strokeColor: "#FF0000",
                        strokeOpacity: 1.0,
                        strokeWeight: 2
                    });

                    that.trailCoords.setMap(that.map);
                });
            }
        }
    }
</script>