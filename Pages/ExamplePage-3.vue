<template>
  <DefaultLayout title="Your experiments">
    <template #header>
      <h1>
        Your experiments
      </h1>
    </template>

    <div
      class="flex flex-col my-[3.188rem] mx-8"
    >
      <div>
        <Button
          class="w-64"
          icon="plus"
          label="Create new experiment"
          @click="showCreateExperimentModal"
        />
      </div>

      <div class="flex justify-between items-center mt-7.5">
        <div class="flex flex-col items-center space-y-4 2xl:flex-row 2xl:space-y-0 2xl:space-x-6">
          <div class="flex items-center space-x-6 w-full">
            <ActionButton
              v-if="experimentsDisplay === 'list'"
              class="opacity-80"
              icon="view-grid"
              label="Card view"
              @click="toggleExperimentsDisplay"
            />
            <ActionButton
              v-else
              class="opacity-80"
              icon="list-view"
              label="List view"
              @click="toggleExperimentsDisplay"
            />
            <ActionButton
              icon="filter"
              label="Filters"
              @click="showExperimentsFiltersModal"
            />
          </div>

          <AppliedFilters
            :filters="filters"
            @remove-filter="removeFilterByName"
            @clear-filters="clearFilters"
          />
        </div>
        <Pagination
          v-if="experiments.data.length > 0"
          :current-page="experiments.meta.current_page"
          :per-page="experiments.meta.per_page"
          :total-items="experiments.meta.total"
          @pagination-page-change="(page) => getExperiments(page)"
        />
      </div>

      <div
        v-if="experimentsDisplay === 'list'"
        class="mt-7.5 space-y-0.5"
      >
        <ExperimentListItem
          v-for="experiment in experiments.data"
          :key="experiment.id"
          :experiment="experiment"
        />
      </div>
      <div
        v-else
        class="grid grid-cols-3 gap-6 mt-7 2xl:grid-cols-5"
      >
        <ExperimentCard
          v-for="experiment in experiments.data"
          :key="experiment.id"
          :experiment="experiment"
        />
      </div>

      <div class="flex justify-end items-end mt-6">
        <Pagination
          v-if="experiments.data.length > 0"
          :current-page="experiments.meta.current_page"
          :per-page="experiments.meta.per_page"
          :total-items="experiments.meta.total"
          @pagination-page-change="(page) => getExperiments(page)"
        />
      </div>
    </div>
  </DefaultLayout>
</template>

<script setup>
import {
  defineProps, ref, inject, watch,
} from 'vue';
import { Inertia } from '@inertiajs/inertia';
import { getFiltersObject } from '@/scripts/utils/helpers';
import DefaultLayout from '~/Layouts/DefaultLayout.vue';
import ActionButton from '~/Shared/Form/ActionButton.vue';
import Button from '~/Shared/Form/Button.vue';
import Pagination from '~/Shared/Pagination.vue';
import ExperimentListItem from '~/Shared/Experiments/ExperimentListItem.vue';
import ExperimentCard from '~/Shared/Experiments/ExperimentCard.vue';
import AppliedFilters from '~/Shared/Filters/AppliedFilters.vue';

const props = defineProps({
  experiments: {
    type: Object,
    required: true,
  },
  statuses: {
    type: Array,
    required: true,
  },
  metrics: {
    type: Array,
    required: true,
  },
  funnelStages: {
    type: Array,
    required: true,
  },
  appliedFilters: {
    type: Object,
    default: null,
  },
});

const modal = inject('$vfm');
const experimentsDisplay = ref(window.localStorage.getItem('experiments_display') ?? 'list');
const filters = ref(props.appliedFilters);

const clearFilters = () => {
  filters.value = null;
};

const getExperiments = (page) => {
  const filterList = getFiltersObject(filters.value);

  Inertia.visit(window.route('.....', {
    ...filterList,
    page,
  }), {
    replace: true,
    preserveScroll: true,
    preserveState: true,
    only: ['experiments', 'filters'],
  });
};

const showCreateExperimentModal = () => {
  modal.show({
    component: 'CreateExperimentModal',
  }, {
    metrics: props.metrics,
    funnelStages: props.funnelStages,
  });
};

const showExperimentsFiltersModal = () => {
  modal.show({
    component: 'ExperimentsFiltersModal',
    on: {
      filtersApplied: (event) => {
        filters.value = event;
      },
    },
  }, {
    filterByDurationEnabled: true,
    statuses: props.statuses,
    metrics: props.metrics,
    funnelStages: props.funnelStages,
    filters: filters.value,
  });
};

// TODO refactor
const removeFilterByName = (key) => {
  if (filters.value[key]) {
    delete filters.value[key];

    if (Object.keys(filters.value).length === 0) {
      filters.value = null;
    }

    getExperiments();
  }
};

const toggleExperimentsDisplay = () => {
  if (experimentsDisplay.value === 'list') {
    experimentsDisplay.value = 'grid';
  } else {
    experimentsDisplay.value = 'list';
  }

  window.localStorage.setItem('experiments_display', experimentsDisplay.value);
};

watch(
  () => filters.value,
  () => getExperiments(),
);
</script>
<style>
@-moz-document url-prefix() {
  .form-button_primary button {
    font-weight: unset;
  }
}
</style>
