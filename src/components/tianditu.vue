<template>
    <button @click="addPolygon">点击添加多边形</button>
    <div>拾取坐标位置{{ position }}</div>
    <div class="bodyAll">
        <div id="map" style="height:100%;width:100%"></div>
    </div>
</template>
<script setup>
import { onMounted, ref } from 'vue';

// 注册地图，添加卫星地图、路面信息
let map, polygons = [], selectedParcel = null;
const zoom = 18;
const initMap = () => {
    const T = window.T; // 全局引入后 T 被注册到 window 里，从这儿拿到 T
    const key = "c45bc5e96a30f5dccce248e4788dbc40";
    const imageURL = "https://t0.tianditu.gov.cn/img_w/wmts?" +
        "SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles" +
        "&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&tk=" + key;
    // 影像注记url           
    const imageURLT = "http://t0.tianditu.gov.cn/cia_w/wmts?" +
        "SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=cia&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles" +
        "&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}" +
        "&tk=" + key;

    const lay = new T.TileLayer(imageURL, { minZoom: 6, maxZoom: 18 });
    const layT = new T.TileLayer(imageURLT, { minZoom: 6, maxZoom: 18 });
    const config = { layers: [lay, layT] };
    map = new T.Map('map', config);
    map.centerAndZoom(new T.LngLat(108.383620, 22.814760), zoom); // 三个参数分别为经度，纬度，缩放等级
    const cp = new T.CoordinatePickup(map, { callback: getLngLat })
    cp.addEvent();
};



let position = ref("")
const getLngLat = (lnglat) => {

    position.value = lnglat.lng.toFixed(10) + "," + lnglat.lat.toFixed(10)

}


//地块测试数据
const parcels = [
    {
        parcel_id: 1,
        farmer_id: 1001,
        location: '张三农场',
        coordinates: { type: 'Point', coordinates: [108.3833700000, 22.8151300000] },
        boundary: {
            type: 'Polygon',
            coordinates: [
                [
                    [108.3833700000, 22.8151300000],
                    [108.3838500000, 22.8150700000],
                    [108.3838000000, 22.8146900000],
                    [108.3834100000, 22.8146900000],
                    [108.3833700000, 22.8151300000]
                ]
            ]
        },
        area: 10.5,
        shape_description: '矩形',
        crop_type: '水稻',
        planting_period: '春季',
        status: '种植中',
        season: '2023/2024',
        created_at: '2023-01-01 12:00:00',
        updated_at: '2023-01-01 12:00:00'
    },
    {
        parcel_id: 2,
        farmer_id: 1002,
        location: '李四农场',
        coordinates: { type: 'Point', coordinates: [108.3838500000, 22.8150700000] },
        boundary: {
            type: 'Polygon',
            coordinates: [
                [
                    [108.3838500000, 22.8150700000],
                    [108.3843500000, 22.8150200000],
                    [108.3843000000, 22.8146400000],
                    [108.3838000000, 22.8146900000],
                    [108.3838500000, 22.8150700000]
                ]
            ]
        },
        area: 8.2,
        shape_description: '矩形',
        crop_type: '小麦',
        planting_period: '秋季',
        status: '闲置',
        season: '2023/2024',
        created_at: '2023-01-01 12:00:00',
        updated_at: '2023-01-01 12:00:00'
    },
    {
        parcel_id: 3,
        farmer_id: 1003,
        location: '王五农场',
        coordinates: { type: 'Point', coordinates: [108.3838000000, 22.8146900000] },
        boundary: {
            type: 'Polygon',
            coordinates: [
                [
                    [108.3838000000, 22.8146900000],
                    [108.3843000000, 22.8146400000],
                    [108.3842500000, 22.8142600000],
                    [108.3837500000, 22.8142900000],
                    [108.3838000000, 22.8146900000]
                ]
            ]
        },
        area: 12.3,
        shape_description: '矩形',
        crop_type: '玉米',
        planting_period: '夏季',
        status: '种植中',
        season: '2023/2024',
        created_at: '2023-01-01 12:00:00',
        updated_at: '2023-01-01 12:00:00'
    },
    {
        parcel_id: 4,
        farmer_id: 1004,
        location: '赵六农场',
        coordinates: { type: 'Point', coordinates: [108.3834100000, 22.8146900000] },
        boundary: {
            type: 'Polygon',
            coordinates: [
                [
                    [108.3834100000, 22.8146900000],
                    [108.3839100000, 22.8146400000],
                    [108.3838600000, 22.8142600000],
                    [108.3833600000, 22.8142900000],
                    [108.3834100000, 22.8146900000]
                ]
            ]
        },
        area: 9.8,
        shape_description: '矩形',
        crop_type: '大豆',
        planting_period: '春季',
        status: '种植中',
        season: '2023/2024',
        created_at: '2023-01-01 12:00:00',
        updated_at: '2023-01-01 12:00:00'
    },
    {
        parcel_id: 5,
        farmer_id: 1005,
        location: '孙七农场',
        coordinates: { type: 'Point', coordinates: [108.3833700000, 22.8151300000] },
        boundary: {
            type: 'Polygon',
            coordinates: [
                [
                    [108.3833700000, 22.8151300000],
                    [108.3838700000, 22.8150800000],
                    [108.3838200000, 22.8147000000],
                    [108.3833200000, 22.8147500000],
                    [108.3833700000, 22.8151300000]
                ]
            ]
        },
        area: 7.6,
        shape_description: '矩形',
        crop_type: '棉花',
        planting_period: '秋季',
        status: '闲置',
        season: '2023/2024',
        created_at: '2023-01-01 12:00:00',
        updated_at: '2023-01-01 12:00:00'
    }
];


//添加多边形
const addPolygon = () => {

    parcels.forEach(parcel => {
        const points = parcel.boundary.coordinates[0].map(coords => new T.LngLat(coords[0], coords[1]));
        const polygon = new T.Polygon(points, {
            color: "blue", weight: 3, opacity: 0.5, fillColor: "#FFFFFF", fillOpacity: 0.5
        });
        polygons.push(polygon);
        map.addOverLay(polygon);
        polygon.addEventListener("click", () => {
            const infoContent = `
    <div class='test' style='margin:0px;'>
        <div style='margin:10px 10px; '>
            <img style='float:left;margin:0px 10px' src='http://lbs.tianditu.gov.cn/images/logo.png' width='101' height='49' title='天安门'/>
            <div style='margin:10px 0px 10px 120px;'>电话 : (010)88187700 <br>地址：北京市顺义区机场东路国门商务区地理信息产业园2号楼天地图大厦</div>
            <input id='keyWord' value='机场' type='text' style='border: 1px solid #cccccc;width: 180px;height: 20px;line-height: 20px;padding-left: 10px;display: block;float: left;'>
            <input style='margin-left:195px;  width: 80px;height: 24px; text-align: center; background: #5596de;color: #FFF;border: none;display: block;cursor: pointer;' type='button' value='周边搜索' onClick="localsearch.searchNearby(document.getElementById('keyWord').value, marker.getLngLat(), radius)">
        </div>
    </div>`;
            const markerInfoWin = new T.InfoWindow(infoContent);
            polygon.openInfoWindow(markerInfoWin);

        });
    })



};

onMounted(() => {
    initMap();
    addPolygon();
});





</script>



<style scoped>
.bodyAll {
    width: 100vw;
    height: 100vh;
}

</style>