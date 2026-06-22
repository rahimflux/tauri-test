<script setup lang="ts">
import { storeToRefs } from "pinia";
import { useDashboardStore } from "@/domains/dashboard/stores/dashboardStore";

const dashboardStore = useDashboardStore();

const {
  evidenceHealth,
  connectionStatus,
  lastSync,
  workspaceName,
  workspaceId,
  incomingFiles,
  groupedDocuments,
  readyForReview,
  rejectedFiles,
  reviewBacklog,
  processingQueue,
  recentActivities,
} = storeToRefs(dashboardStore);

function uploadEvidence(): void {
  dashboardStore.uploadEvidence();
}
</script>

<template>
  <div class="space-y-6">
    <!-- SYSTEM STATUS -->
    <h1 style="color: red; font-size: 40px">BUILD TEST 22-JUNE</h1>
    <div class="grid gap-4 lg:grid-cols-3">
      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="text-sm font-medium text-slate-500">Evidence Health</h3>

        <p class="mt-4 text-5xl font-bold text-emerald-600">
          {{ evidenceHealth }}%
        </p>

        <p class="mt-2 text-sm text-slate-500">
          Successfully Processed Evidence
        </p>
      </div>

      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="text-sm font-medium text-slate-500">Connection Status</h3>

        <p class="mt-4 text-xl font-semibold text-emerald-600">
          {{ connectionStatus }}
        </p>

        <p class="mt-2 text-sm text-slate-500">Last Sync: {{ lastSync }}</p>
      </div>

      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="text-sm font-medium text-slate-500">Workspace</h3>

        <p class="mt-4 text-lg font-semibold">
          {{ workspaceName }}
        </p>

        <p class="mt-2 text-sm text-slate-500">
          {{ workspaceId }}
        </p>
      </div>
    </div>

    <!-- KPI -->

    <div class="grid gap-4 md:grid-cols-2 xl:grid-cols-4">
      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="text-sm text-slate-500">Incoming Files</h3>

        <p class="mt-3 text-4xl font-bold">
          {{ incomingFiles }}
        </p>
      </div>

      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="text-sm text-slate-500">Grouped Documents</h3>

        <p class="mt-3 text-4xl font-bold">
          {{ groupedDocuments }}
        </p>
      </div>

      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="text-sm text-slate-500">Ready For Review</h3>

        <p class="mt-3 text-4xl font-bold text-emerald-600">
          {{ readyForReview }}
        </p>
      </div>

      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="text-sm text-slate-500">Rejected Files</h3>

        <p class="mt-3 text-4xl font-bold text-red-500">
          {{ rejectedFiles }}
        </p>
      </div>
    </div>

    <!-- UPLOAD + PIPELINE -->

    <div class="grid gap-4 lg:grid-cols-2">
      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="text-lg font-semibold">Upload Evidence</h3>

        <p class="mt-2 text-sm text-slate-500">
          Upload invoices, receipts and supporting documents.
        </p>

        <button
          class="mt-6 rounded-xl bg-emerald-600 px-5 py-3 text-white"
          @click="uploadEvidence"
        >
          Upload Files
        </button>
      </div>

      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="mb-5 text-lg font-semibold">Processing Pipeline</h3>

        <div class="space-y-4">
          <div>
            <div class="mb-1 flex justify-between">
              <span>Incoming</span>
              <span>{{ incomingFiles }}</span>
            </div>

            <div class="h-2 rounded bg-slate-200">
              <div class="h-2 w-full rounded bg-blue-500"></div>
            </div>
          </div>

          <div>
            <div class="mb-1 flex justify-between">
              <span>Grouped</span>
              <span>{{ groupedDocuments }}</span>
            </div>

            <div class="h-2 rounded bg-slate-200">
              <div class="h-2 w-8/12 rounded bg-indigo-500"></div>
            </div>
          </div>

          <div>
            <div class="mb-1 flex justify-between">
              <span>Review</span>
              <span>{{ readyForReview }}</span>
            </div>

            <div class="h-2 rounded bg-slate-200">
              <div class="h-2 w-4/12 rounded bg-emerald-500"></div>
            </div>
          </div>

          <div>
            <div class="mb-1 flex justify-between">
              <span>Rejected</span>
              <span>{{ rejectedFiles }}</span>
            </div>

            <div class="h-2 rounded bg-slate-200">
              <div class="h-2 w-2/12 rounded bg-red-500"></div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ACTIVITY + QUEUE -->

    <div class="grid gap-4 lg:grid-cols-2">
      <div class="rounded-2xl bg-white dark:bg-slate-800 shadow-sm">
        <div class="border-b border-slate-200 p-5">
          <h3 class="font-semibold">Recent Activity</h3>
        </div>

        <div
          v-for="activity in recentActivities"
          :key="activity.fileName"
          class="border-b border-slate-100 p-4 last:border-b-0"
        >
          <div class="flex items-center justify-between">
            <div>
              <p class="font-medium">
                {{ activity.fileName }}
              </p>

              <p class="text-sm text-slate-500">
                {{ activity.status }}
              </p>
            </div>

            <span class="text-xs text-slate-500">
              {{ activity.time }}
            </span>
          </div>
        </div>
      </div>

      <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
        <h3 class="font-semibold">AI Processing Queue</h3>

        <div class="mt-5 space-y-3">
          <div
            v-for="file in processingQueue"
            :key="file"
            class="rounded-xl bg-slate-100 dark:bg-slate-700 p-4"
          >
            {{ file }}
          </div>
        </div>
      </div>
    </div>

    <!-- REVIEW BACKLOG -->

    <div class="rounded-2xl bg-white dark:bg-slate-800 p-6 shadow-sm">
      <h3 class="text-sm text-slate-500">Human Review Backlog</h3>

      <p class="mt-3 text-5xl font-bold text-amber-600">
        {{ reviewBacklog }}
      </p>

      <p class="mt-2 text-sm text-slate-500">
        Document groups awaiting manual verification.
      </p>
    </div>
  </div>
</template>
