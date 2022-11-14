<template>
    <div class="tabs">
        <div class="tab" v-for="tab in tabs" :key="tab.id" :class="{ 'active': tab.active }" @click="toggleTab(tab.id)">
            {{ tab.title }}
        </div>
    </div>
</template>

<script>
export default {
    name: 'AppTabs',
    props: {
        tabs: Array,
    },
    methods: {
        toggleTab(tabId) {
            this.tabs.forEach(tab => {
                if (tab.id === tabId) {
                    tab.active = true;
                } else {
                    tab.active = false;
                }
            })
        }
    }
}
</script>

<style scoped lang="scss">
@import '../assets/scss/variables.scss';

.tabs {
    display: flex;
    position: sticky;
    top: 39px;
    right: 0;
    left: 0;
    z-index: 30;

    .tab {
        padding: 10px 0;
        height: inherit;
        flex: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        color: black;
        text-transform: uppercase;
        font-weight: bold;
        letter-spacing: 4px;
        background-color: $light-green;
        cursor: pointer;

        &.active {
            transition: all 0.5s;
            color: white;
            background-color: darken($light-green, 20%);
        }

        /* &:hover {
        background-color: darken($light-green, 10%);
    }*/
    }
}
</style>
