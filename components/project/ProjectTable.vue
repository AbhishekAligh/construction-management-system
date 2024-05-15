<template>
    <div class="shadow-[rgba(99,99,99,0.2)_0px_2px_8px_0px] my-24 mx-12 p-8 rounded-xl">
        <UTable :columns="columns" :rows="data" />
    </div>
</template>


<script setup lang="ts">

const columns = [
    {
        key: 'project',
        label: 'Project'
    },
    {
        key: 'crewType',
        label: 'Crew Type'
    },
    {
        key: 'hourlyRate',
        label: 'Hourly Rate'
    },
    {
        key: 'quantity',
        label: 'Quantity'
    },
    {
        key: 'hours',
        label: 'Hours',
        sortable: true
    },
    {
        key: 'cost',
        label: 'Cost/CrewType($)',
        sortable: true
    }
]

const props = defineProps<{
    data: [{}]
}>()
</script>