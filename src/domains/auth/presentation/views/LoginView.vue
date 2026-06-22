<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";
import { authService } from "@/domains/auth/application/authService";
import { useAuthStore } from "@/domains/auth/stores/authStore";

const authStore = useAuthStore();
const router = useRouter();

const email = ref("admin@fluxbooks.com");
const password = ref("Password@123");

const errorMessage = ref("");
const loading = ref(false);

async function handleLogin(): Promise<void> {
  console.log("CLICKED");

  try {
    loading.value = true;

    console.log("BEFORE LOGIN");

    const user = await authService.login({
      email: email.value,
      password: password.value,
    });

    console.log("AFTER LOGIN");

    authStore.login(user);

    console.log("LOCAL STORAGE:", localStorage.getItem("fluxbooks-auth"));

    await router.push("/dashboard");

    console.log("ROUTER PUSH COMPLETE");
  } catch (error) {
    console.error(error);

    errorMessage.value =
      error instanceof Error ? error.message : "Login failed";
  } finally {
    loading.value = false;
  }
}
</script>

<template>
  <div
    class="flex min-h-screen items-center justify-center bg-slate-100 px-6 dark:bg-slate-950"
  >
    <div
      class="w-full max-w-md rounded-3xl border border-slate-200 bg-white p-8 shadow-2xl dark:border-slate-800 dark:bg-slate-900"
    >
      <div class="mb-8">
        <div
          class="mb-4 flex h-12 w-12 items-center justify-center rounded-xl bg-emerald-600 font-bold text-white"
        >
          FB
        </div>

        <h1
          class="text-3xl font-bold text-slate-900 dark:text-white"
        >
          FluxBooks Tray
        </h1>

        <p
          class="mt-2 text-slate-500 dark:text-slate-400"
        >
          AI Powered Bookkeeping Client
        </p>
      </div>

      <!-- keep the remaining form exactly as-is -->
      <div class="space-y-4">
        <div>
          <label class="mb-2 block text-sm text-slate-500 dark:text-slate-200"> Email </label>
      
          <input
            v-model="email"
            type="email"
            class="w-full rounded-xl border border-slate-300 bg-slate-200 dark:bg-slate-800 dark:border-slate-800 px-4 py-3 dark:text-white outline-none focus:border-emerald-500"
          />
        </div>
      
        <div>
          <label class="mb-2 block text-sm text-slate-500 dark:text-slate-200"> Password </label>
      
          <input
            v-model="password"
            type="password"
            class="w-full rounded-xl border border-slate-300 bg-slate-200 dark:bg-slate-800 dark:border-slate-800 px-4 py-3 dark:text-white outline-none focus:border-emerald-500"
          />
        </div>
      
        <button
          class="w-full rounded-xl bg-emerald-600 py-3 font-semibold text-white transition hover:bg-emerald-500"
          :disabled="loading"
          @click="handleLogin"
        >
          {{ loading ? "Signing In..." : "Sign In" }}
        </button>
      
        <p v-if="errorMessage" class="text-sm text-red-400">
          {{ errorMessage }}
        </p>
      </div>
      
      <div class="mt-8 border-t border-slate-800 pt-4 text-xs text-slate-500 dark:text-slate-400">
        Demo Credentials: admin@fluxbooks.com / Password@123
      </div>
    </div>
  </div>
</template>
