<template>
    <div class="shadow-[rgba(99,99,99,0.2)_0px_2px_8px_0px] my-8 mx-12 rounded-xl p-4">
        <UForm :state="state" class="space-y-4" @submit="onSubmit" :schema="schema">
            <UFormGroup label="Project Name" name="name" required>
                <UInput v-model="state.name" />
            </UFormGroup>
            <div class="flex gap-x-4">
                <UFormGroup label="Crew Type" name="crewType" required>
                    <USelect placeholder="Select Crew Type" v-model="state.crewType" :options="crewCatalog"
                        option-attribute="name" />
                </UFormGroup>
                <UFormGroup label="Quantity" name="quantity" required>
                    <UInput type="number" v-model="state.quantity" />
                </UFormGroup>
                <UFormGroup label="Start Time" name="startTime" required>
                    <UInput type="time" v-model="state.startTime" step="3600" />
                </UFormGroup>
                <UFormGroup label="End Time" name="endTime" required>
                    <UInput type="time" v-model="state.endTime" @change="calculateHours" step="3600" />
                </UFormGroup>
                <UFormGroup label="Total Hours" name="totalHours" required>
                    <UInput type="number" disabled v-model="state.totalHours" />
                </UFormGroup>
                <UButton type="submit" class="mt-6">
                    Submit
                </UButton>

            </div>
            <UButton label="Rate Card" @click="isOpen = true" />
            <UModal v-model="isOpen">
                <div class="p-4">
                    <CrewTable />
                </div>
            </UModal>

        </UForm>
    </div>
    <ProjectTable :data="data" />
</template>

<script setup lang="ts">
import { z } from 'zod'
import type { FormSubmitEvent } from '#ui/types'

const schema = z.object({
    name: z.string(),
    crewType: z.string(),
    quantity: z.number().min(1, "Has to be atleast one"),
    startTime: z.string(),
    endTime: z.string(),
    totalHours: z.number()
})
type Schema = z.output<typeof schema>

const state = reactive({
    name: undefined,
    crewType: 0,
    quantity: 0,
    startTime: undefined,
    endTime: undefined,
    totalHours: 0

})
const isOpen = ref(false)
const isOpen1 = ref(false)
const data = ref([{}])
const crewCatalog = [{ name: 'Foremen', value: 20, }, { name: 'Operator', value: 30 }, { name: 'Labourer', value: 10 }]


async function onSubmit(event: FormSubmitEvent<Schema>) {
    calculateHours()
    console.log((+event.data.crewType) * event.data.quantity * event.data.totalHours)
    data.value.push(
        {
            project: event.data.name,
            crewType: (+event.data.crewType) == +20 ? 'Foremen' : +event.data.crewType === 10 ? 'Labourer' : 'Operator',
            hourlyRate: +event.data.crewType,
            quantity: +event.data.quantity,
            hours: event.data.totalHours,
            cost: (+event.data.crewType) * event.data.quantity * event.data.totalHours,
        }
    )
    // console.log(data.value[1]);
}

const calculateHours = () => {
    if (state.startTime) {
        let [startTime, x] = state.startTime.split(":")
        let [endTime, y] = state.endTime.split(":")
        state.totalHours = endTime - startTime
        console.log(startTime);
        return state.totalHours
    }
}
</script>