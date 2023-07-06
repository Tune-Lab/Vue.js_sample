<template>
    <div class="card">
        <div class="card-header">
            <div class="d-flex flex-sm-row align-items-sm-center flex-column align-items-start">
                <h5 class="card-title mb-0 text-dark fs-20 fw-semibold flex-grow-1">{{ cardTitle }}</h5>
                <div v-if="showCardHeader" class="flex-shrink-0 py-2">
                    <slot name="card-header" />
                </div>
            </div>
        </div>

        <div v-if="showSearch" class="card-body px-3" style="padding-left: 0">
            <slot name="search" />
        </div>

        <div v-if="textEmpty.length > 0 && showSearch" class="px-3 text-muted mt-2 fs-14">
            <p>{{ textEmpty }}</p>
        </div>

        <div v-else class="card-body mt-4">
            <div class="table-responsive table-card">
                <table v-if="textEmpty.length === 0" class="table align-middle table-nowrap">
                    <thead class="table-light text-muted">
                    <tr>
                        <slot name="header" />
                        <slot name="additional-header" />

                        <th
                            v-if="!showHeaders"
                            v-for="(headerItem, headerIndex) in headers"
                            :key="headerIndex"
                            class="sort"
                            scope="col"
                        >
                            {{ headerItem.name }}
                            <span
                                v-if="headerItem.name === 'Date'"
                                class="cursor-pointer"
                                @click="emit('changeDateType')"
                            >
                                <i class="ri-time-line text-success"></i>
                            </span>
                        </th>

                    </tr>
                    </thead>

                    <tbody class="list form-check-all">
                        <slot name="body" />
                    </tbody>

                    <tfoot class="table-light">
                        <slot name="tfoot" />
                    </tfoot>
                </table>

                <p v-else class="text-muted mt-2 fs-14" style="margin-left: 10px">{{ textEmpty }}</p>
            </div>

            <div class="d-flex justify-content-center justify-content-sm-end mt-3">
                <slot name="footer" />
            </div>
        </div>
    </div>
</template>

<script setup>
import { computed, defineProps, useSlots } from 'vue';


defineProps({
    textEmpty: {
        type: String,
        default: ''
    },
    headers: {
        type: Array,
        default: []
    },
    cardTitle: {
        type: String,
        default: ''
    }
});

const slots = useSlots();
const emit = defineEmits(['changeDateType']);

const showCardHeader = computed(() => {
    return !!slots['card-header']
});

const showHeaders = computed(() => {
    return !!slots['header']
});

const showSearch = computed(() => {
    return !!slots['search']
});
</script>

<style scoped>

</style>
