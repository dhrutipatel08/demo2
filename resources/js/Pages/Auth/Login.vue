<script setup>
import Checkbox from '@/Components/Checkbox.vue';
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';

defineProps({
    canResetPassword: {
        type: Boolean,
    },
    status: {
        type: String,
    },
});

const form = useForm({
    email: '',
    password_or_otp: '',
});

const submit = () => {
    form.post(route('login'), {
        onSuccess: () => {
            window.location = route('dashboard');
        },
        onFinish: () => form.reset('password_or_otp'),
    });
};

const requestOtp = () => {
    form.post(route('otp.request'), {
        preserveScroll: true,
        onSuccess: () => alert('OTP sent to your email!'),
    });
};
</script>

<template>
    <GuestLayout>
        <Head title="Log in" />

        <div v-if="status" class="mb-4 text-sm font-medium text-green-600">
            {{ status }}
        </div>

        <form @submit.prevent="submit">
            <div>
                <InputLabel for="email" value="Email" />

                <TextInput
                    id="email"
                    type="email"
                    class="mt-1 block w-full"
                    v-model="form.email"
                    required
                    autofocus
                    autocomplete="username"
                />

                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <div class="mt-4">
                <InputLabel for="password_or_otp" value="Password or OTP" />
                <div class="flex gap-2">
                    <TextInput
                        id="password_or_otp"
                        type="text"
                        class="mt-1 block w-full"
                        v-model="form.password_or_otp"
                        required
                        autocomplete="current-password"
                    />
                    <PrimaryButton type="button" @click="requestOtp" :disabled="!form.email || form.processing">
                        Request OTP
                    </PrimaryButton>
                </div>
                <InputError class="mt-2" :message="form.errors.password_or_otp" />
            </div>

            <div class="mt-4 flex items-center justify-center">
                <PrimaryButton
                    :class="{ 'opacity-25': form.processing }"
                    :disabled="form.processing"
                >
                    Log in
                </PrimaryButton>
            </div>
        </form>
    </GuestLayout>
</template>
