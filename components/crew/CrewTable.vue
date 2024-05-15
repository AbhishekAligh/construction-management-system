<script setup lang="ts">
const columns = [{
    key: 'id',
    label: 'ID'
}, {
    key: 'crewType',
    label: 'Crew Type'
    },
    {
    key: 'hourlyRate',
    label: 'Hourly Rate($)',
    sortable: true
}]
const crewMember = [{
    id: 1,
    crewType: 'Foremen',
    hourlyRate: '20'
}, {
    id: 2,
    crewType: 'Operator',
    hourlyRate: '30'
}, {
    id: 3,
    crewType: 'Labourer',
    hourlyRate: '10'

}]
</script>

<template>
    <UTable :columns="columns" :rows="crewMember"
        class="shadow-[rgba(99,99,99,0.2)_0px_2px_8px_0px] my-24 mx-12 rounded-xl" />
</template>
