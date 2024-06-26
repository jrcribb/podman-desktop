<script lang="ts">
import { CloseButton, LinearProgress, Link } from '@podman-desktop/ui-svelte';
import { router } from 'tinro';

import { currentPage, lastPage } from '../../stores/breadcrumb';

export let title: string;
export let showBreadcrumb = true;
export let inProgress = false;

export function close(): void {
  router.goto($lastPage.path);
}

function handleKeydown(e: KeyboardEvent) {
  if (e.key === 'Escape') {
    close();
    e.preventDefault();
  }
}
</script>

<svelte:window on:keydown="{handleKeydown}" />

<div class="flex flex-col w-full h-full shadow-pageheader">
  <div class="flex flex-row w-full h-fit px-5 pt-4" class:pb-3.5="{inProgress}" class:pb-4="{!inProgress}">
    <div class="flex flex-col w-full h-fit">
      {#if showBreadcrumb}
        <div class="flex flew-row items-center text-sm text-[var(--pd-content-breadcrumb)]">
          <Link aria-label="back" on:click="{() => router.goto($lastPage.path)}" title="Go back to {$lastPage.name}"
            >{$lastPage.name}</Link>
          <div class="mx-2">&gt;</div>
          <div class="grow font-extralight" aria-label="name">{$currentPage.name}</div>
          <CloseButton class="justify-self-end" on:click="{() => router.goto($lastPage.path)}" />
        </div>
      {/if}
      <div class="flex flex-row items-center pt-1">
        {#if $$slots.icon}
          <div class="pr-3 text-[var(--pd-content-header-icon)]">
            <slot name="icon" />
          </div>
        {/if}
        <h1 aria-label="{title}" class="grow text-xl first-letter:uppercase text-[var(--pd-content-header)]">
          {title}
        </h1>
        <div class="flex items-center space-x-3">
          {#if $$slots.actions}
            <div class="flex flex-nowrap justify-self-end pl-3 space-x-2">
              <slot name="actions" />
            </div>
          {/if}
          {#if !showBreadcrumb}
            <CloseButton class="justify-self-end" on:click="{() => router.goto($lastPage.path)}" />
          {/if}
        </div>
      </div>
    </div>
  </div>
  {#if inProgress}
    <LinearProgress />
  {/if}
  {#if $$slots.tabs}
    <div class="flex flex-row px-2 border-b border-[var(--pd-content-divider)]">
      <slot name="tabs" />
    </div>
  {/if}
  <div class="flex w-full h-full bg-[var(--pd-content-bg)] overflow-auto">
    <slot name="content" />
  </div>
</div>
