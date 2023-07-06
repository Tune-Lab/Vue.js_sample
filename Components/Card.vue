<template>
    <div class="card">
        <div class="card-header d-flex flex-column flex-sm-row align-items-sm-center align-items-start">
            <h4 v-if="cardTitle && cardTitle.length > 0" class="card-title mb-0 flex-grow-1 fs-20">{{ cardTitle }}</h4>
            <div v-if="showCardHeader" :class="[classMap, 'flex-shrink-0 col-12 col-sm-auto py-sm-0 py-2']">
                <slot name="card-header" />
            </div>
        </div>

        <div class="card-body">
            <slot name="card-body" />
        </div>
    </div>
</template>

<script setup>
import { computed, useSlots, defineProps } from "vue";


const props = defineProps({
    cardTitle: {
        type: String,
        default: ''
    },
    classMap: {
        type: String,
        default: ''
    }
});

const slots = useSlots();

const showCardHeader = computed(() => {
    return !!slots['card-header']
});

</script>

<style scoped>

</style>
