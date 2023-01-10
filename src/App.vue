<template>
    <q-layout view="lHh Lpr lFf">
        <q-header>
            <q-toolbar>
                <q-btn flat dense round @click="leftDrawerOpen = !leftDrawerOpen" aria-label="Menu" icon="menu" />
            </q-toolbar>
            <div class="q-px-lg q-pt-xl q-mb-md">
                <!-- <div class="text-h3">Todo</div> -->
                <div class="text-subtitle1">{{ todaysDate }}</div>
            </div>
            <q-img src="./statics/background.jpg" alt="image-background" class="header-image absolute-top" />
        </q-header>

        <q-drawer v-model="leftDrawerOpen" show-if-above bordered class="bg-grey-2" :width="250" :breakpoint="600">
            <q-list>
                <q-item-label header>Essential Links</q-item-label>
                <q-item clickable v-ripple>
                    <q-item-section avatar>
                        <q-icon name="list" />
                    </q-item-section>
                    <q-item-section>
                        <q-item-label>Todo</q-item-label>
                    </q-item-section>
                </q-item>
                <q-item clickable v-ripple>
                    <q-item-section avatar>
                        <q-icon name="help" />
                    </q-item-section>
                    <q-item-section>
                        <q-item-label>Help</q-item-label>
                    </q-item-section>
                </q-item>
            </q-list>
        </q-drawer>

        <q-page-container>
            <router-view>
            </router-view>
        </q-page-container>
    </q-layout>
</template>

<script>
import { computed, ref } from 'vue'
import {date} from "quasar";

export default {
    name: 'LayoutDefault',
    setup() {

         // a computed ref
        const todaysDate = computed(() => {
            let timeStamp = Date.now();
            return date.formatDate(timeStamp, 'dddd D MMMM')
        })

        return {
            leftDrawerOpen: ref(false), todaysDate
        }
    }

  
}
</script>

<style lang="scss">
.header-image {
    height: 100%;
    z-index: -1;
    opacity: 0.2;
    filter: grayscale(100%);
}
</style>