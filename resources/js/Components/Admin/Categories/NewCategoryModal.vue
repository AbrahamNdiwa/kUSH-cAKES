<script setup lang="ts">

import DialogModal from "@/Components/DialogModal.vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";
import {ref, watch} from "vue";
import TextInput from "@/Components/TextInput.vue";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import {Link, useForm} from "@inertiajs/vue3";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import Checkbox from "@/Components/Checkbox.vue";

const emit = defineEmits(['close']);

const props = defineProps({
    show: {
        type: Boolean,
        default: false,
    },
});
const showModal = ref(props.show);

// Watch for changes in the `show` prop to update the modal state
watch(() => props.show, (newVal) => {
  showModal.value = newVal;
});
// Close the modal and emit the custom `close` event
const closeModal = () => {
  showModal.value = false;
  emit('close');
};
const form = useForm({
    name: '',
    description: '',

});
const submit = () => {
    form.post(route('admin.categories.store'), {
        onSuccess: () => {
            form.reset()
            closeModal()
        },
    });
};
</script>

<template>
    <DialogModal
        :show="showModal"
        :max-width="'2xl'"
        :closeable="true"
        @close="closeModal"
    >
      <template #title>
                New Category
            </template>

            <template #content>
               <form @submit.prevent="submit" :disabled="form.processing">
            <div>
                <InputLabel for="name" value="Name" />
                <TextInput
                    id="name"
                    v-model="form.name"
                    type="text"
                    class="mt-1 block w-full"
                    required
                    autofocus
                    autocomplete="name"
                />
                <InputError class="mt-2" :message="form.errors.name" />
            </div>

            <div class="mt-4">
                <InputLabel for="Description" value="Description" />
                <TextInput
                    id="description"
                    v-model="form.description"
                    type="text"
                    class="mt-1 block w-full"
                    required
                    autocomplete="description"
                />
                <InputError class="mt-2" :message="form.errors.description" />
            </div>


            <div class="flex justify-between mt-4">
                <PrimaryButton class="" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                    Save
                </PrimaryButton>
                <SecondaryButton @click="closeModal">
                    Cancel
                </SecondaryButton>
            </div>
        </form>
            </template>
        <template #footer>



            </template>
    </DialogModal>
</template>
