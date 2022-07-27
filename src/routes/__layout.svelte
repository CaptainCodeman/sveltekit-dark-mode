<script lang="ts" context="module">
  import "../app.css";
</script>

<script lang="ts">
  import Theme from "$lib/Theme.svelte"
</script>

<!-- simple layout with top navigation to host our theme switcher -->
<div class="min-h-full">
  <nav class="bg-slate-100 dark:bg-slate-700 border-b border-slate-200 dark:border-slate-900 transition-dark">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex justify-between h-16">
        <div class="flex">
          <div class="flex-shrink-0 flex items-center">
            <img class="h-8 w-auto block dark:hidden transition-dark" src="https://tailwindui.com/img/logos/workflow-mark-indigo-600.svg" alt="Workflow">
            <img class="h-8 w-auto hidden dark:block transition-dark" src="https://tailwindui.com/img/logos/workflow-mark-indigo-300.svg" alt="Workflow">
          </div>
        </div>
        <div class="flex items-center">
          <Theme />
        </div>
      </div>
    </div>
  </nav>

  <main class="py-10">
    <div class="max-w-6xl mx-auto px-6 lg:px-8">
      <slot></slot>
    </div>
  </main>
</div>
