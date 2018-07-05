<template>
    <div id="lots-of-map-components"></div>
</template>

<script>

export default {
    props : {
        meta : {
            type : Object,
            default : () => ( {
                center: [116.397428, 39.90923],
                zoom: 3,
                resizeEnable: true,
            } )
        },
        polygons : {
            type : Array,
            default : () => [],
        },
        labels : {
            type : Array,
            default : () => [],
        },
        dots : {
            type : Array,
            default : () => [],
        },
        pins : {
            type : Array,
            default : () => [],
        },
    },

    data : function() {
        return {
            mapObj : Object,
        }
    },

    mounted () {
        var sc_node = document.createElement( 'script' );
        sc_node.src = "https://webapi.amap.com/maps?v=1.4.7&key=3103eab320f18f668712168411c2557c";
        document.head.insertBefore( sc_node, document.head.lastChild );
        sc_node.onload = () => {
            this.mapObj = new AMap.Map( 'lots-of-map-components', this.meta );

            let change_handle = ( e ) => {
                let bound = this.mapObj.getBounds(),
                    southWest = bound.getSouthWest(),
                    northEast = bound.getNorthEast();
                this.$emit( 'map-move', {
                    southWestLng : southWest.getLng(),
                    southWestLat : southWest.getLat(),
                    northEastLng : northEast.getLng(),
                    northEastLat: northEast.getLat(),
                } )
            }
            this.mapObj
                .on( 'moveend', change_handle )

            this.drawPolygons();
            this.drawPins();
            this.moveCenter();
        }
    },

    methods : {
        drawPolygons () {
            let ols = this.mapObj.getAllOverlays( 'polygon' );
            this.mapObj.remove( ols );
            
            this.polygons.forEach( pol => {
                let polygon = new AMap.Polygon( {
                    zIndex: pol.zIndex || 1,
                    path: pol.path,
                    bubble : pol.bubble || false,
                    cursor : pol.cursor || undefined,
                    strokeColor: pol.strokeColor || '#fff',
                    strokeOpacity: pol.strokeOpacity || 1,
                    strokeWeight: pol.strokeWeight || 3,
                    fillColor: pol.fillColor || '#fff',
                    fillOpacity: pol.fillOpacity || .5,
                    extData: pol.id,
                    strokeStyle : pol.strokeStyle || "solid",
                } )

                polygon.setMap( this.mapObj );
                polygon.on( 'click', ( e ) => {
                    this.$emit( 'click-polygon', e );
                    this.$emit( 'click', e );
                } ).on( 'dblclick', ( e ) => {
                    this.$emit( 'dblclick-polygon', e );
                    this.$emit( 'dblclick', e );
                } ).on( 'change', () => {
                    this.$emit( 'change-polygon', polygon );
                } )
            } );
        },

        drawLabels () {

        },

        drawDots () {

        },

        drawPins () {
            let ols = this.mapObj.getAllOverlays( 'marker' );
            this.mapObj.remove( ols );

            this.pins.forEach( p => {
                let icon = new AMap.Icon( {
                    image : p.image || undefined,
                } );

                let marker = new AMap.Marker( {
                    position: [p.lng, p.lat],
                    icon,
                    extData: p.id,
                } );

                marker.setMap( this.mapObj );

                marker.on( 'click', ( e ) => {
                    this.$emit( 'click-pin', e );
                    this.$emit( 'click', e );
                } ).on( 'dblclick', ( e ) => {
                    this.$emit( 'dblclick-pin', e );
                    this.$emit( 'dblclick', e );
                } ).on( 'click', () => {
                    this.$emit( 'change-pin', marker );
                } );
            } )
        },

        moveCenter ( type ) {
            let ols = this.mapObj.getAllOverlays( type );
            this.mapObj.setFitView( ols );
        }
    },
    watch : {
        polygons ( newObj, oldObj ) {
            let polygon_list = this.mapObj.getAllOverlays( 'polygon' );

            for ( let np of newObj ) {
                polygon_list.find( x=> x.getExtData() == np.id ).setOptions( {
                    zIndex: np.zIndex || 1,
                    path: np.path,
                    bubble : np.bubble || false,
                    cursor : np.cursor || undefined,
                    strokeColor: np.strokeColor || '#fff',
                    strokeOpacity: np.strokeOpacity || 1,
                    strokeWeight: np.strokeWeight || 3,
                    fillColor: np.fillColor || '#fff',
                    fillOpacity: np.fillOpacity || .5,
                    extData: np.id,
                    strokeStyle : np.strokeStyle || "solid",
                } )
            }
        },
        pins ( newObj, oldObj ) {
            let pin_list = this.mapObj.getAllOverlays( 'marker' );
            for ( let np of newObj ) {
                let icon = new AMap.Icon( {
                    image : np.image || undefined,
                } );

                let pin = pin_list.find( x=> x.getExtData() == np.id );
                pin.setIcon( icon );
                pin.setPosition( [np.lng, np.lat ] );
            }
        }
    }
}

</script>

<style>
#lots-of-map-components {
    width: 100%;
    height: 100%;
    position: relative;
}
</style>

