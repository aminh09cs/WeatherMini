<template>
    <section class="container" ref="bgweb">
        <div class="weather-location">
            <div class="weather-location-left">
                <img src="../assets/images/Vector.png" alt="local">
                <p>Fortaleza</p>
                <img src="../assets/images/show.png" alt="arrow">
            </div>
            <div class="weather-location-right">
                <img src="../assets/images/bell.png" alt="bell">

            </div>
        </div>
        <div class="weather-img">
            <img src="../assets/images/sun.png" alt="Sun" class="wea" ref="bgsun">
            <div class="weather-info">
                <h1>{{ show_temp["temperature_2m"] }}{{ temperature_2m_unit }}</h1>
                <p>Precipipations</p>
                <span>{{ max_temp.temperature_2m }}&deg;C</span>
                <span>{{ min_temp.temperature_2m }}&deg;C</span>
            </div>
            <ul class="weather-param" ref="bgparam">
                <li class="weather-param-info">
                    <span><img src="../assets/images/rain.png" alt=""></span>
                    <span>{{ show_temp["rain"] + rain_unit }}</span>
                </li>
                <li class="weather-param-info">
                    <span><img src="../assets/images/temp.png" alt=""></span>
                    <span>{{ show_temp["relativehumidity_2m"] + relativehumidity_2m_unit }}</span>
                </li>
                <li class="weather-param-info">
                    <span><img src="../assets/images/wind.png" alt=""></span>
                    <span>{{ show_temp["windspeed_10m"] + windspeed_10m_unit }}</span>
                </li>
            </ul>
            <div class="scroll">
                <div class="weather-date" ref="bgdate">
                    <div class="weather-date-top">
                        <p>{{ currentDay }}</p>
                        <p>{{ currentDate }}</p>
                    </div>
                    <div class="weather-date-bottom">
                        <div class="weather-date-bottom-info" v-for="(item, i) in dataDay" :key="i"
                            @click="onShowTemp(item)">
                            <p>{{ item["temperature_2m"] }}&deg;C</p>
                            <img src="../assets/images/wea.png" alt="">
                            <p>{{ item["time"].slice(-5) }}</p>
                        </div>
                    </div>
                </div>
                <div class="weather-weekly" ref="bgweekly">
                    <div class="weather-weekly-top">
                        <p class="weather-weekly-top-title">Next Forecast</p>
                        <div>
                            <img src="../assets/images/calendar.png" alt="calendar">
                        </div>
                    </div>
                    <div class="weather-weekly-bottom">
                        <ul class="weather-weekly-bottom-week">
                            <li class="day-weather" v-for="(day, i) in dataWeek" :key="i">
                                <div>{{ day["day"] }}</div>
                                <div>
                                    <img src="../assets/images/rain_week.png" alt="">
                                </div>
                                <div>
                                    {{ day["min_temp"] }}&#8451;
                                    {{ day["max_temp"] }}&#8451;
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>
<script setup>
import axios from 'axios'
import { ref, onMounted } from "vue";
//show day/night screen
let bgweb = ref(null);
let bgparam = ref(null);
let bgdate = ref(null);
let bgsun = ref(null);
let bgweekly = ref(null);
// current date data
let currentDate = ref(null);
let currentDay = ref(null);

//weather forcast data UNIT
let time_unit = ref("");
let temperature_2m_unit = ref("");
let relativehumidity_2m_unit = ref("");
let rain_unit = ref("");
let windspeed_10m_unit = ref("");


//data in a day
let dataDay = ref([]);
let dataWeek = ref([
    {
        day: "Monday",
        min_temp: "1.3",
        max_temp: "2.7"
    },
    {
        day: "Tuesday",
        min_temp: "2.1",
        max_temp: "-0.5"
    },
    {
        day: "Wednesday",
        min_temp: "1.1",
        max_temp: "1.9"
    },
    {
        day: "Thursday",
        min_temp: "4.6",
        max_temp: "2.7"
    },
    {
        day: "Friday",
        min_temp: "2",
        max_temp: "1.2"
    },
    {
        day: "Sartuday",
        min_temp: "2.5",
        max_temp: "1.7"
    },
    {
        day: "Sunday",
        min_temp: "1.3",
        max_temp: "2.3"
    },
]);
let data = ref({});


let min_temp = ref({});
let max_temp = ref({});
let show_temp = ref({});

//check day/night
const checkDayNight = () => {
    let currentDate = new Date();
    let hour = currentDate.toTimeString().split(" ")[0].slice(0, 2);

    switch (true) {
        //day
        case (+hour >= 6 && +hour <= 18):
            bgweb.value.style.background = "linear-gradient(167.44deg, #29B2DD 0%, #33AADD 47.38%, #2DC8EA 100%)";
            bgparam.value.style.background = "rgba(255, 255, 255, 0.2)";
            bgdate.value.style.background = "rgba(255, 255, 255, 0.2)";
            //bgsun.value.src = "../assets/images/sun.png";
            bgweekly.value.style.background = "rgba(255, 255, 255, 0.2)";
            break;
        //night
        case ((+hour >= 0 && +hour <= 5) || (+hour >= 19 && +hour <= 23)):
            bgweb.value.style.background = "linear-gradient(167.44deg, #08244F 0%, #134CB5 47.38%, #0B42AB 100%)";
            bgparam.value.style.background = "rgb(11, 56, 134)";
            bgdate.value.style.background = "rgb(11, 56, 134)";
            //bgsun.value.src = "../assets/images/sun.png";
            bgweekly.value.style.background = "rgb(11, 56, 134)";

            break;
        default:
            break;
    }

}
//convert month num to String 1-Jan, 2-Fre
const toMonthName = (date) => {
    return date.toLocaleString('en-US', { month: 'long' });
}

const toDayName = (date) => {
    return date.toLocaleString('en-us', { weekday: 'long' });
}
//get Date
const checkDateNow = () => {
    const date = new Date();

    let day = date.getDate();
    let month = toMonthName(date);
    let year = date.getFullYear();
    day = day.toString().padStart(2, '0');
    //May 3rd, 2016
    currentDate.value = `${month} ${day}, ${year}`;
    //Tuesday
    currentDay.value = toDayName(date);
}

//get Data Weekly and get day now
const getDataDay = () => {
    //let dataWeekly = {};
    let date = new Date();
    let day = date.getDate();
    let month = (date.getMonth() + 1).toString();
    let year = (date.getFullYear()).toString();
    day = day.toString().padStart(2, '0');
    //ex: 2022-12-06
    let dataDay_ = [];
    let obj = {}
    date = `${year}-${month}-${day}`;
    data.value["hourly"]["time"].forEach((item, i) => {
        if (date === item.slice(0, 10)) {
            obj = {
                "time": data.value["hourly"]["time"][i],
                "temperature_2m": data.value["hourly"]["temperature_2m"][i],
                "relativehumidity_2m": data.value["hourly"]["relativehumidity_2m"][i],
                "rain": data.value["hourly"]["rain"][i],
                "windspeed_10m": data.value["hourly"]["windspeed_10m"][i]
            }
            dataDay_.push(obj);
        }
    })
    dataDay.value = dataDay_;
}
//show temp by hour
const showTempByHour = () => {
    let date = new Date();
    let hour = date.getHours().toString().padStart(2, '0');
    let obj = {}
    dataDay.value.forEach((item) => {
        if (item["time"].slice(11, 13) === hour) {
            show_temp.value = item["temperature_2m"];
            obj = {
                "temperature_2m": item["temperature_2m"],
                "relativehumidity_2m": item["relativehumidity_2m"],
                "rain": item["rain"],
                "windspeed_10m": item["windspeed_10m"],

            }
        }
    })
    show_temp.value = obj;
}
const getData = async () => {
    await axios.get("https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&hourly=temperature_2m,relativehumidity_2m,rain,windspeed_10m")
        .then((res) => {
            if (res && res.data) {
                data.value = res.data;
            }

            //set unit
            time_unit.value = data.value["hourly_units"]["time"];
            temperature_2m_unit.value = data.value["hourly_units"]["temperature_2m"];
            relativehumidity_2m_unit.value = data.value["hourly_units"]["relativehumidity_2m"];
            rain_unit.value = data.value["hourly_units"]["rain"];
            windspeed_10m_unit.value = data.value["hourly_units"]["windspeed_10m"];

            //set data weekly
            getDataDay();
            //Find min/max temp
            min_temp.value = dataDay.value.reduce(function (prev, curr) {
                return prev.temperature_2m < curr.temperature_2m ? prev : curr;
            })
            max_temp.value = dataDay.value.reduce(function (prev, curr) {
                return prev.temperature_2m > curr.temperature_2m ? prev : curr;
            })

            //show temp by hour
            showTempByHour();
        })
        .catch((e) => {
            alert(e);
        })
}
onMounted(() => {
    checkDayNight();
    checkDateNow();
    getData();
})
const onShowTemp = (item) => {
    show_temp.value.temperature_2m = item["temperature_2m"];
}
</script>

<style lang="scss">
.container {
    width: 423px;
    height: 900px;
    margin: 0 auto;
    background: linear-gradient(167.44deg, #08244F 0%, #134CB5 47.38%, #0B42AB 100%);
    border-radius: 40px;
    padding: 70px;

    .weather-location {
        display: flex;
        justify-content: space-between;

        &-left {
            display: flex;
            align-items: center;
            gap: 10px;

            p {
                font-style: normal;
                font-weight: 600;
                font-size: 16px;
                line-height: 21px;
            }
        }

        &-right {
            position: relative;

            &::after {
                position: absolute;
                top: 0;
                right: 0;

                content: "";
                display: block;
                background-color: #FF7C7C;
                width: 5px;
                height: 5px;
                border-radius: 50%;
            }
        }
    }

    .weather-img {
        width: 284px;
        height: 150px;

        img {
            width: 100%;
            height: 100%;
        }

        .wea {
            animation: move 5s infinite;
            animation-direction: alternate;
        }

        .weather-info {
            text-align: center;
            font-weight: 400;
            margin: 0 auto;

            h1 {
                font-style: normal;
                font-weight: 600;
                font-size: 64px;
            }

            span:nth-of-type(1) {
                margin-right: 10px;

            }
        }

        .weather-param {
            display: flex;
            justify-content: space-between;

            background-color: rgb(11, 56, 134);
            box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.25);
            border-radius: 20px;
            padding: 10px 40px;
            margin-top: 20px;
            font-size: 12px;

            &-info {
                display: flex;
                gap: 7px;

                span {
                    display: block;
                    width: 15px;
                    height: 15px;

                    img {
                        width: 100%;
                        height: 100%;
                    }
                }
            }
        }

        .scroll {
            overflow-y: scroll;
            height: 400px;

            .weather-date {
                background-color: rgb(11, 56, 134);
                box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.25);
                padding: 20px;
                margin-top: 20px;
                border-radius: 20px;


                &-top {
                    display: flex;
                    justify-content: space-between;

                    margin-bottom: 15px;

                    p:nth-of-type(1) {
                        font-weight: 700;
                        font-size: 18px;
                        line-height: 24px;
                    }

                    p:nth-of-type(2) {
                        font-weight: 400;
                        font-size: 16px;
                        line-height: 24px;
                    }
                }

                &-bottom {
                    display: flex;
                    justify-content: space-between;
                    overflow: auto;

                    &-info {
                        display: flex;
                        flex-direction: column;
                        padding: 20px 5px;
                        transition: border 1s;
                        flex-basis: 25%;

                        &:hover {
                            border: 1px solid rgba(80, 150, 255, 1);
                            border-radius: 20px;
                            cursor: pointer;
                        }

                        img {
                            width: 50px;
                            height: 50px;
                        }
                    }
                }
            }

            .weather-weekly {
                background: rgba(255, 255, 255, 0.2);
                box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.25);

                border-radius: 20px;
                padding: 20px;
                margin-top: 20px;

                .weather-weekly-top {
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    margin-bottom: 10px;

                    &-title {
                        font-weight: 700;
                        font-size: 20px;
                        line-height: 24px;
                    }

                    img {
                        cursor: pointer;
                        object-fit: cover;
                        width: 100%;
                        height: 100%;
                    }
                }

                .weather-weekly-bottom {
                    &-week {
                        display: flex;
                        justify-content: space-around;
                        flex-direction: column;

                        .day-weather {
                            display: flex;
                            justify-content: space-around;
                            align-items: center;

                            div:nth-of-type(1) {
                                width: 90px;
                                font-weight: 700;
                                font-size: 16px;
                                line-height: 22px;
                            }
                        }
                    }
                }
            }
        }

        ::-webkit-scrollbar {
            display: none;
        }
    }
}

@keyframes move {
    0% {
        transform: translate(-10px);
    }

    100% {
        transform: translate(10px);
    }
}
</style>