<template>
    <div>
        <Head :title="title" />

        <VerticalLayout v-if="layoutType === 'vertical'" :layout="layoutType">
            <slot />
        </VerticalLayout>
    </div>
</template>
<script setup>
import { getCurrentInstance, watch } from "vue";
import { usePage, Head, router } from "@inertiajs/vue3";
import { isEmpty } from "lodash";
import VerticalLayout from "./VerticalLayout.vue";


defineProps({
    title: {
        type: String,
        default: ''
    }
});

const { proxy } = getCurrentInstance();
const layoutType = usePage().props.auth.data.settings.layoutType;


watch(
    () => usePage().props.flash,
    (flashMessage) => {
        if (!isEmpty(flashMessage) && flashMessage.type !== undefined) {
            proxy.$toast(flashMessage);
        }
    }
);
</script>