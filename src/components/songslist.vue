<template lang="">
    <a-layout-sider
        breakpoint="sm"
        collapsed-width="0"
        @collapse="onCollapse"
        @breakpoint="onBreakpoint"
        width="300"
        style="background: #fff"
    >
        <a-menu
            v-model:selectedKeys="selected_keys"
            v-model:openKeys="open_keys"
            mode="inline"
            style="height: 100%"
            @select="selected"
        >
            <a-sub-menu key="sub1">
                <template #title>
                    <span> 网易云音乐接口</span>
                </template>

                <a-menu-item
                    style="font-size: 9px"
                    v-for="(song, index) in songs"
                    :key="(index + 1).toString()"
                >
                    <span class="artist_name">{{ song.artists[0].name }}</span>
                    <span class="song_name">{{ song.name }}</span>
                </a-menu-item>
            </a-sub-menu>
        </a-menu>
    </a-layout-sider>
</template>
<script>
export default {
    name: "songslist",
    components: {},
    props: ["selected_keys", "open_keys", "songs"],
    data() {
        return {};
    },
    setup() {
        const onCollapse = (collapsed, type) => {
            //console.log(collapsed, type);
        };

        const onBreakpoint = (broken) => {
            //console.log(broken);
        };
        return {
            onCollapse,
            onBreakpoint,
        };
    },

    methods: {
        selected(song) {
            //console.log(song);
            this.$emit("selected", this.selected_keys);
        },
    },
};
</script>
<style lang="css">
.artist_name {
    opacity: 0.8;
}

.artist_name::after {
    content: " - ";
    opacity: 0.5;
}

.song_name {
    font-weight: bold;
}
</style>
