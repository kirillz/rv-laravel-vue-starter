<template>
    <ul class="space-y-2">
        <template v-for="item in $props.state.mainMenu">
            <li v-if="item.hasOwnProperty('subitems') && item.subitems.length > 0 && isEnabled(item, $props.user, $props.type)">
                <button @click.prevent="state.currentExpandedMenuItem  ? state.currentExpandedMenuItem = null : state.currentExpandedMenuItem = item" type="button" class="transition duration-75 group w-full flex items-center p-2 text-base font-normal text-white font-semibold rounded-lg hover:text-theme-300 hover:bg-theme-800 dark:hover:bg-theme-800" :class="isActive(item) ? 'bg-theme-800' : ''">
                    <Icon :name="item.icon" class="mr-2 text-3xl pl-2 -mt-1"/>
                    <span class="flex-1 text-left" v-html="item.name"></span>
                    <Icon :name="JSON.stringify(state.currentExpandedMenuItem) === JSON.stringify(item) ? 'angle-up' : 'angle-down'" class="mr-2 text-3xl pl-2 -mt-1"/>
                </button>
                <ul id="dropdown-example" class="py-2 space-y-2" :class="JSON.stringify(state.currentExpandedMenuItem) === JSON.stringify(item) ? '' : 'hidden'">
                    <template v-for="subitem in item.subitems">
                        <li v-if="isEnabled(subitem, $props.user, $props.type)">
                            <router-link :to="subitem.to ? subitem.to : '#'" @click.prevent="subitem.hasOwnProperty('onClick') ?? subitem.onClick" class="flex items-center p-2 pl-11 w-full text-base font-normal text-white font-semibold rounded-lg hover:text-theme-300 hover:bg-theme-800 dark:hover:bg-theme-800" :class="isActive(subitem) ? 'bg-theme-800' : ''">
                                {{ subitem.name }}
                                <span class="sr-only" v-html="subitem.name"></span>
                            </router-link>
                        </li>
                    </template>

                </ul>
            </li>
            <li v-else-if="isEnabled(item, $props.user, $props.type)">
                <a v-if="item.hasOwnProperty('onClick') && !item.to" href="#" class="flex items-center p-2 text-base font-normal text-white font-semibold rounded-lg hover:text-theme-300 hover:bg-theme-800 dark:hover:bg-theme-800" @click.prevent="item.onClick">
                    <Icon :name="item.icon" class="mr-2 text-3xl pl-2 -mt-1"/>
                    <span class="ml-1" v-html="item.name"></span>
                    <span class="sr-only" v-html="item.name"></span>
                    <span v-if="item.hasOwnProperty('label') && item.label" class="inline-flex justify-center items-center px-2 ml-3 text-sm font-medium text-gray-800 bg-gray-200 rounded-full dark:bg-gray-700 dark:text-gray-300" v-html="item.label"></span>
                </a>
                <router-link v-else :to="item.to ? item.to : '#'" class="flex items-center p-2 text-base font-normal text-white font-semibold rounded-lg hover:text-theme-300 hover:bg-theme-800 dark:hover:bg-theme-800" :class="isActive(item) ? 'bg-theme-800' : ''">
                    <Icon :name="item.icon" class="mr-2 text-3xl pl-2 -mt-1"/>
                    <span class="ml-1" v-html="item.name"></span>
                    <span class="sr-only" v-html="item.name"></span>
                    <span v-if="item.hasOwnProperty('label') && item.label" class="inline-flex justify-center items-center px-2 ml-3 text-sm font-medium text-gray-800 bg-gray-200 rounded-full dark:bg-gray-700 dark:text-gray-300" v-html="item.label"></span>
                </router-link>
            </li>
        </template>
    </ul>
</template>

<script>

import {defineComponent} from "vue";
import {useRouter} from "vue-router";
import Icon from "@/views/utils/Icon";

export default defineComponent({
    name: "Menu",
    components:{
        Icon
    },
    props: {
        state: {
            type: Object,
            default: {},
        },
        type: {
            type: String,
            default: '',
        },
        user: {
            type: Object,
            default: {},
        },
    },
    setup(props) {

        const router = useRouter();

        function isActive(obj) {
            let currentPath = router.currentRoute.value.path;
            let isActiveMainItem = obj.to === currentPath;
            let isActiveSubItem = false;
            if(obj.hasOwnProperty('subitems')) {
                for(let i in obj.subitems) {
                    if(obj.subitems[i].to === currentPath) {
                        isActiveSubItem = true;
                        break;
                    }
                }
            }
            return isActiveMainItem || isActiveSubItem;
        }

        function isEnabled(obj, user, type) {

            if (!obj) {
                return false;
            }
            let roleCheck = (false !== obj.showIfRole) ? parseInt(obj.showIfRole) === parseInt(user.role) : true;
            let totalEnabledSubItems = 0;
            if(obj.hasOwnProperty('subitems')) {
                for(var i in obj.subitems) {
                    if(isEnabled(obj.subitems[i], user, type)) {
                        totalEnabledSubItems++;
                    }
                }
            } else {
                totalEnabledSubItems = 1;
            }

            if (type === 'desktop') {
                return roleCheck && obj.showDesktop && totalEnabledSubItems > 0;
            } else if (type === 'mobile') {
                return roleCheck && obj.showMobile && totalEnabledSubItems > 0;
            }
            return false;
        }

        return {
            isEnabled,
            isActive
        }
    }
});
</script>
