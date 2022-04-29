<template>
    <a-layout>
        <a-page-header class="header"
        :title="song"/> 
        <a-layout-content style="padding: 0 50px">
            <div style="margin: 40px 0"></div>
            <a-layout style="padding: 24px 0; background: #fff">
                <songslist
                    @selected="selected"
                    :songs="info_163"
                    :selected_keys="selectedKeys"
                    :open_keys="openKeys"
                />
                <a-layout-content
                    :style="{ padding: '0 24px', minHeight: '280px' }"
                >
                    <songinfo
                        :song_info="song_info"
                        :lyric="lyric"
                        :picUrl="picUrl"
                        v-if="Object.keys(song_info).length > 0"
                        ref="songinfo"
                    />
                </a-layout-content>
            </a-layout>
        </a-layout-content>
        <a-layout-footer style="text-align: center"> </a-layout-footer>
    </a-layout>
</template>
<script>
import { defineComponent, ref } from "vue";
import songslist from "./components/songslist.vue";
import songinfo from "./components/songinfo";
import fetchJsonp from "fetch-jsonp";
export default defineComponent({
    components: {
        songslist,
        songinfo,
    },
    data() {
        return {
            song: "",
            info_163: [],
            selectedKeys: ["1"],
            openKeys: ["sub1"],
            song_info: {},
            lyric: "",
            picUrl: "",
        };
    },
    setup() {
        return {};
    },
    methods: {
        song_change(song) {
            console.log("Changed song: ", song);
            this.song = song;
            var options = {
                headers: {
                    "User-Agent": "okhttp/4.4.0",
                    "Accept-Encoding": "gzip",
                },
            };
            // music.163.com Api 1
            // fetchJsonp(
            //     `http://s.music.163.com/search/get/?type=1&s=${song}&limit=15&offset=0`,
            //     options
            // )
            //     .then(function (response) {
            //         return response.json();
            //     })
            //     .then((json) => {
            //         // console.log("parsed json", json);
            //         this.info_163 = json.result.songs;
            //         this.song_info = this.info_163[0];
            //         console.log(this.$refs.songinfo);

            //         var get = ()=> {
            //             if (this.$refs.songinfo) {
            //                 this.$refs.songinfo.get_lyric(this.song_info.id);
            //             } else {
            //                 setTimeout(get, 250);
            //             }
            //         };

            //         get();

            //         //this.$refs.songinfo.get_lyric(this.song_info.id);
            //         //console.log(this);
            //         //this.selectedKeys = ["1"];
            //         //console.log(json.result.songs);
            //     })
            //     .catch(function (ex) {
            //         //console.log("parsing failed", ex);
            //     });
            this.$nextTick(() => {
            window.opener.postMessage(
                {
                    type: "get_songs",
                    song: song
                },
                "*"
            );
        });
        },
        selected(selectedKeys) {
            this.lyric = ""
            this.song_info = this.info_163[selectedKeys - 1];
            console.log("init");
            this.$refs.songinfo.get_lyric(this.song_info.id);
        },
    },
    mounted() {
        window.song_change = this.song_change;
        window.addEventListener("message", (event) => {
            if (event.data.type == "song_change") {
                this.song_change(event.data.song);
            }
            if (event.data.type == "song_lyric") {
                this.lyric = event.data.lyric;
                this.picUrl = event.data.picUrl;
            }
            if (event.data.type == "song_list") {
                this.info_163 = event.data.info;
                this.song_info = this.info_163[0];
            //         console.log(this.$refs.songinfo);

                    var get = ()=> {
                        if (this.$refs.songinfo) {
                            this.$refs.songinfo.get_lyric(this.song_info.id);
                        } else {
                            setTimeout(get, 250);
                        }
                    };

                    get();
            }
        });
        this.$nextTick(() => {
            window.opener.postMessage(
                {
                    type: "ready",
                },
                "*"
            );
        });
    },
});
</script>
<style>
#components-layout-demo-top-side .logo {
    float: left;
    width: 120px;
    height: 31px;
    margin: 16px 24px 16px 0;
    background: rgba(255, 255, 255, 0.3);
}

.ant-row-rtl #components-layout-demo-top-side .logo {
    float: right;
    margin: 16px 0 16px 24px;
}

.site-layout-background {
    background: #fff;
}
</style>
