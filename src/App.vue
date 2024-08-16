<template>
  <div class="map">
    <GoogleMap
      :api-key="MAP_API_KEY"
      class="search_map"
      :center="currentLocation"
      :zoom="10"
      :fullscreen-control="false"
      :styles="mapStyle"
      :mapTypeControl="false"
      :streetViewControl="false"
      ref="mapRef"
      style="width: 100%; height: 100vh"
    >
      <InfoWindow
        v-if="infowindow"
        :options="{ position: infowindowCenter }"
        @click="toShop"
      >
        <div class="info-bubble">
          <p>{{ infoShop.name }}</p>
          <p>{{ infoShop.address }}</p>
        </div>
      </InfoWindow>
      <MarkerCluster>
        <CustomMarker
          v-for="(shop, i) in stores"
          :options="{
            position: {
              lat: parseFloat(shop.latitude),
              lng: parseFloat(shop.longitude),
            },
          }"
          :key="i"
          @mousedown="closeExistBubble"
          @mouseup="showBubble($event, shop)"
        >
          <div class="map__marker" style="position: absolute; top: 40%; left: 5%">
            <div class="map__marker-photo">
              <img class="map__marker-photo-img" :src="image" alt="" width="" height="" />
            </div>
          </div>
        </CustomMarker>
      </MarkerCluster>
    </GoogleMap>

    <nav class="map__nav">
        <ul class="map__menu">
          <li class="map__menu-item">
            <button class="map__menu-location" type="button">現在地</button>
          </li>
          <li class="map__menu-item">
            <RouterLink class="map__menu-list" to="/search/list">一覧</RouterLink>
          </li>
        </ul>
        <div class="map__slide">
          <Carousel>
            <Slide v-for="(shop, index) in stores" :key="index">
              <div class="map__slide-item">
                <div class="map__slide-content">
                  <div class="map__slide-photo">
                    <img class="map__slide-photo-img" :src="image" alt="" width="" height="" />
                  </div>
                  <div class="map__slide-info">
                    <ul class="map__slide-category">
                      <li class="map__slide-category-item">category</li>
                    </ul>
                    <p class="map__slide-name">name</p>
                  </div>
                </div>
                <p class="map__slide-address">address</p>
              </div>
            </Slide>
          </Carousel>
        </div>
      </nav>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { GoogleMap, MarkerCluster, CustomMarker, InfoWindow } from "vue3-google-map";
import { useRouter, useRoute } from 'vue-router';

import 'vue3-carousel/dist/carousel.css';
import { Carousel, Slide } from 'vue3-carousel';

const MAP_API_KEY = "";
const currentLocation = { lat: 43.063577, lng: 141.351886 };
const infowindow = ref(false);
const infowindowCenter = ref({ lat: 43.063577, lng: 141.351886 });
const infoShop = ref({
    id: null,
    name: '',
    address: '',
});
const router = useRouter();
const route = useRoute();
const closeExistBubble = () => {
  infowindow.value = false;
};
const mapRef = ref(null);
const image = "https://cdn.pixabay.com/photo/2016/02/19/15/46/labrador-retriever-1210559_640.jpg";

const mapStyle = [
  {
    featureType: "all",
    stylers: [{ lightness: -10 }],
  },
  {
    elementType: "labels.icon",
    stylers: [
      {
        visibility: "off",
      },
    ],
  },
  {
    elementType: "labels.text",
    stylers: [
      {
        visibility: "on",
      },
    ],
  },
];

const stores = [
  {
    id: 76,
    name: "rew",
    company_name: "421rer",
    url: null,
    code: "11111",
    zipcode: "8508501",
    prefecture_name: "長崎県",
    address: "長崎市新地町",
    block_address: "sd324",
    building_name: "123sdasd",
    business_hours: null,
    regular_holiday: null,
    phone: "012-222-2222",
    introduction: null,
    access_way: null,
    homepage: null,
    isFavorite: null,
    latitude: "32.74070370",
    longitude: "129.87430510",
    prefecture: {
      id: 1,
      name: "北海道",
    },
    municipality: {
      id: 9,
      name: "札幌市手稲区",
    },
  },
  {
    id: 56,
    name: "青山",
    company_name: "青森",
    url: null,
    code: "123456",
    zipcode: "6508567",
    prefecture_name: "兵庫県",
    address: "神戸市中央区",
    block_address: "5-10-1",
    building_name: "2",
    business_hours: null,
    regular_holiday: null,
    phone: "08021087457",
    introduction: null,
    access_way: null,
    homepage: null,
    isFavorite: null,
    latitude: "34.69214200",
    longitude: "135.17897880",
    prefecture: {
      id: 28,
      name: "兵庫県",
    },
    municipality: {
      id: 1311,
      name: "神戸市中央区",
    },
  },
  {
    id: 46,
    name: "laitest5",
    company_name: "laitest6",
    url: "https://autowave.runsystem.work/storage/stores/1697616874b4cee90c5c6c43979b764e88bd149479.png",
    code: "0000000012",
    zipcode: "2550000",
    prefecture_name: "",
    address: "中郡大磯町",
    block_address: "〒255-0000神奈川県",
    building_name: null,
    business_hours: "test2",
    regular_holiday: "test3",
    phone: "06785646743",
    introduction: "test5",
    access_way: "test6",
    homepage: "https://autowave.front.runsystem.work/shop/register",
    isFavorite: null,
    latitude: "35.30683690",
    longitude: "139.31136420",
    prefecture: {
      id: 10,
      name: "群馬県",
    },
    municipality: {
      id: 506,
      name: "安中市",
    },
  },
  {
    id: 10,
    name: "vustore2",
    company_name: "vucompany2",
    url: "https://autowave.runsystem.work/storage/stores/1697081649d0bbdbb2c57f4c899ac2b3091b056f34.jpg",
    code: "bb445599",
    zipcode: "1900100",
    prefecture_name: "",
    address: "あきる野市",
    block_address: "〒190-0100東京都",
    building_name: null,
    business_hours: "lam tu 5h sang toi 12h dem",
    regular_holiday: "nghi chu nhat",
    phone: "0931945951",
    introduction: "title",
    access_way: "title",
    homepage: null,
    isFavorite: null,
    latitude: "35.72894130",
    longitude: "139.29410940",
    prefecture: {
      id: 2,
      name: "青森県",
    },
    municipality: {
      id: 195,
      name: "三沢市",
    },
  },
  {
    id: 20,
    name: "lai_store 名前　fullsize",
    company_name: "company 企業名（公開用）",
    url: "https://autowave.runsystem.work/storage/stores/16971888539f08712f8ece4fbbab376e94397c4dfa.jpg",
    code: "00008",
    zipcode: "1960000",
    prefecture_name: "",
    address: "昭島市",
    block_address: "〒196-0000東京都",
    building_name: "test",
    business_hours: "test",
    regular_holiday: "test",
    phone: "0456456456456",
    introduction: "test",
    access_way: "test",
    homepage: "https://autowave.front.runsystem.work/shop/register",
    isFavorite: null,
    latitude: "35.70569590",
    longitude: "139.35363330",
    prefecture: {
      id: 14,
      name: "神奈川県",
    },
    municipality: {
      id: 740,
      name: "横浜市磯子区",
    },
  },
  {
    id: 24,
    name: "test 1",
    company_name: "test 2",
    url: "https://autowave.runsystem.work/storage/stores/1697422656a1db9df2a9f34e9397529837a8d76552.jpg",
    code: "00006",
    zipcode: "1960000",
    prefecture_name: "",
    address: "昭島市",
    block_address: "〒196-0000東京都",
    building_name: null,
    business_hours: null,
    regular_holiday: null,
    phone: "056756756567",
    introduction: null,
    access_way: null,
    homepage: null,
    isFavorite: null,
    latitude: "35.70569590",
    longitude: "139.35363330",
    prefecture: {
      id: 7,
      name: "福島県",
    },
    municipality: {
      id: 371,
      name: "二本松市",
    },
  },
  {
    id: 32,
    name: "storelaitt",
    company_name: "laitt",
    url: "https://autowave.runsystem.work/storage/stores/169753458419e3ae9afad84874a901f5acb30a57af.jpg",
    code: "10",
    zipcode: "1960000",
    prefecture_name: "",
    address: "昭島市",
    block_address: "〒196-0000東京都",
    building_name: null,
    business_hours: null,
    regular_holiday: null,
    phone: "80866745654",
    introduction: null,
    access_way: null,
    homepage: null,
    isFavorite: null,
    latitude: "35.70569590",
    longitude: "139.35363330",
    prefecture: {
      id: 6,
      name: "山形県",
    },
    municipality: {
      id: 339,
      name: "尾花沢市",
    },
  },
  {
    id: 57,
    name: "laitest store1",
    company_name: "laitest store1",
    url: null,
    code: "00000001",
    zipcode: "1960000",
    prefecture_name: "東京都",
    address: "昭島市",
    block_address: "〒196-0000東京都",
    building_name: null,
    business_hours: "1",
    regular_holiday: "2",
    phone: "0567567567",
    introduction: "5",
    access_way: "6",
    homepage: null,
    isFavorite: null,
    latitude: "35.70569590",
    longitude: "139.35363330",
    prefecture: {
      id: 1,
      name: "北海道",
    },
    municipality: {
      id: 83,
      name: "空知郡奈井江町",
    },
  },
  {
    id: 75,
    name: "店舗ヴィ",
    company_name: "企業ヴィ",
    url: "https://autowave.runsystem.work/storage/stores/171741002591848d3b805045d7b99ff055bbcd6f7e.jpg",
    code: "99999",
    zipcode: "2410821",
    prefecture_name: "神奈川県",
    address: "横浜市旭区二俣川",
    block_address: "tesst",
    building_name: "tesst",
    business_hours: "毎週月曜日から土曜日まで",
    regular_holiday: "日曜日",
    phone: "0904526643",
    introduction: "紹介文紹介文紹介文紹介文",
    access_way: "駅から5分歩いて",
    homepage: null,
    isFavorite: null,
    latitude: "35.46081400",
    longitude: "139.53436360",
    prefecture: {
      id: 14,
      name: "神奈川県",
    },
    municipality: {
      id: 745,
      name: "横浜市旭区",
    },
  },
  {
    id: 68,
    name: "teststore1",
    company_name: "teststore1",
    url: null,
    code: "11111",
    zipcode: "1858501",
    prefecture_name: "",
    address: "国分寺市戸倉",
    block_address: "asdqwe",
    building_name: null,
    business_hours: null,
    regular_holiday: null,
    phone: "0123456789",
    introduction: null,
    access_way: null,
    homepage: null,
    isFavorite: null,
    latitude: "35.70923220",
    longitude: "139.45422680",
    prefecture: {
      id: 4,
      name: "宮城県",
    },
    municipality: {
      id: 268,
      name: "仙台市泉区",
    },
  },
];

const toShop = () => {
  if (infoShop.value.id) {
    router.push({
      path: `/search/${infoShop.value.id}/detail`,
      query: {
        mobile_number: route.query.mobile_number,
      },
    });
  }
};

const showBubble = (e, shop) => {
    infoShop.value.id = shop.id;
    infoShop.value.name = shop.name;
    infoShop.value.address = `${shop.prefecture_name}${shop.address}${shop.block_address}`;
    infowindowCenter.value = {
        lat: parseFloat(shop.latitude),
        lng: parseFloat(shop.longitude),
    }
    infowindow.value = true;
}
</script>


<style>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.gm-style .gm-style-iw-c + .gm-style-iw-tc, .gm-style .gm-style-iw-c {
    top: -50px;
}

.gm-style .gm-style-iw-c {
    padding: 12px!important;
    max-width: 300px!important;
    background-color: black!important;
    color: white!important;
    border-radius: 5px!important;
    cursor: pointer;
}

.info-bubble {
    font-family: Helvetica, Arial, sans-serif, "Hiragino Kaku Gothic ProN", "Hiragino Sans", "Yu Gothic", "YuGothic";
    font-size: 13px;
    line-height: 1;
}

.gm-style .gm-style-iw-d {
    overflow: auto!important;
}

.gm-style .gm-style-iw-c + .gm-style-iw-tc::after {
    background: black;
}

.gm-style .gm-style-iw-c .gm-ui-hover-effect>span {
    background-color: white;
}

.poi-info-window.gm-style div {
    background-color: black;
    color: white;
}

.poi-info-window.gm-style > div > a {
    background-color: black;
}

body {
  width: 100vw;
  height: 100vh;
  margin: 0;
  padding: 0;
}


.map {
  position: relative;
  height: 100svh;
  overflow: hidden;
}
.map__figure {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(199, 199, 199, 0.3);
  font-size: 18px;
  color: rgba(10, 10, 10, 0.3);
}
.map__marker {
  position: relative;
  width: 72px;
  height: 82px;
  padding: 6px;
  background: url(@/assets/img/search/marker.svg) no-repeat center;
  background-size: cover;
  filter: drop-shadow(0 0 10px rgba(0, 0, 0, 0.2));
}
.map__marker.is-active {
  width: 78px;
  height: 88px;
  padding: 9px;
  background-image: url(@/assets/img/search/marker_active.svg);
}
.map__marker-photo {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  overflow: hidden;
}
.map__marker-photo-img {
  width: 100%;
  height: 100%;
  -o-object-fit: cover;
     object-fit: cover;
}
.map__nav {
  position: fixed;
  bottom: 16px;
  left: 50%;
  transform: translateX(-50%);
  width: 100vw;
}
.map__menu {
  display: grid;
  gap: 16px;
  width: 64px;
  margin-block-end: 16px;
  margin-inline-start: auto;
  margin-inline-end: 8px;
}
.map__menu-location {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 64px;
  height: 64px;
  padding: 6px 0 0;
  border-radius: 50%;
  background: #fff;
  box-shadow: 0 3px 6px rgba(87, 67, 42, 0.08);
  border: 2px solid #eaeaea;
  text-align: center;
  font-size: 12px;
  font-weight: bold;
  color: #0a0a0a;
  line-height: 1;
}
.map__menu-location:before {
  content: "";
  width: 32px;
  height: 32px;
  background: url(@/assets/img/search/location.svg) no-repeat center;
  background-size: cover;
}
.map__menu-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 64px;
  height: 64px;
  padding: 6px 0 0;
  border-radius: 50%;
  background: #5cc0cf;
  box-shadow: 0 3px 6px rgba(87, 67, 42, 0.08);
  border: 2px solid #fff;
  text-align: center;
  font-size: 13px;
  color: #fff;
  line-height: 1;
}
.map__menu-list:before {
  content: "";
  width: 32px;
  height: 32px;
  background: url(@/assets/img/search/list.svg) no-repeat center;
  background-size: cover;
}
.map__slide {
  padding: 0 24px;
}
.map__slide-item {
  width: 100%;
  padding: 16px;
  border-radius: 16px;
  background: #fff;
  box-shadow: 0 3px 6px rgba(87, 67, 42, 0.08);
}
.map__slide-content {
  display: grid;
  grid-template-columns: 72px 1fr;
  gap: 8px;
  width: 100%;
  margin-block-end: 8px;
}
.map__slide-photo {
  width: 72px;
  height: auto;
  aspect-ratio: 1/1;
}
.map__slide-photo-img {
  width: 100%;
  height: 100%;
  -o-object-fit: cover;
     object-fit: cover;
}
.map__slide-category {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 8px;
  margin-block-end: 8px;
}
.map__slide-category-item {
  display: inline-flex;
  justify-content: center;
  align-items: center;
  height: 20px;
  padding: 0 8px;
  border-radius: 4px;
  background: #fff;
  border: 1px solid #ef8202;
  font-size: 12px;
  color: #ef8202;
}
.map__slide-name {
  margin-block: calc((1em - 1lh) / 2);
  text-align: left;
  font-size: 16px;
  font-weight: bold;
  line-height: 1.4;
}
.map__slide-address {
  display: flex;
  gap: 4px;
  margin-block: calc((1em - 1lh) / 2);
  text-align: left;
}
.map__slide-address:before {
  content: "";
  flex: 0 0 auto;
  width: 16px;
  height: 16px;
  /* margin-block-start: calc((1em - 1lh + 16px) / 2); */
  background: url(@/assets/img/search/address.svg) no-repeat center;
  background-size: cover;
}
.map__slide .carousel__viewport {
  overflow: visible !important;
}
.map__slide .carousel__slide {
  padding: 0 4px;
}
.map__slide .carousel__slide--active .map__slide-item {
  border: 3px solid #ef8202;
}/*# sourceMappingURL=style.css.map */
</style>