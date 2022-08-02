<script lang="ts">
	import { onMount } from 'svelte'

	// indicate if we're in dark mode or not
	let dark: boolean

	// hide the control until we've decided what the intial mode is
	let hidden = true

	onMount(() => {
		// use the existence of the dark class on the html element for the initial value
		dark = document.documentElement.classList.contains('dark')

		// show UI controls
		hidden = false

		// listen for changes so we auto-adjust based on system settings
		const matcher = window.matchMedia('(prefers-color-scheme: dark)')
		matcher.addEventListener('change', handleChange)
		return () => matcher.removeEventListener('change', handleChange)
	})

	function handleChange({ matches: dark }: MediaQueryListEvent) {
		// only set if we haven't overridden the theme
		if (!localStorage.theme) {
			setMode(dark)
		}
	}

	function toggle() {
		setMode(!dark)
	}

	function setMode(value: boolean) {
		dark = value

		// update page styling
		if (dark) {
			document.documentElement.classList.add('dark')
		} else {
			document.documentElement.classList.remove('dark')
		}

		// store the theme as a local override
		localStorage.theme = dark ? 'dark' : 'light'

		// if the toggled-to theme matches the system defined theme, clear the local override
		// this effectively provides a way to override or revert to "automatic" setting mode
		if (window.matchMedia(`(prefers-color-scheme: ${localStorage.theme})`).matches) {
			localStorage.removeItem('theme')
		}
	}
</script>

<svelte:head>
	<!-- set dark mode class based on user preference / device settings (in head to avoid FOUC) -->
	<script>
		if (localStorage.theme === 'dark' || (!localStorage.theme && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
			document.documentElement.classList.add('dark')
		} else {
			document.documentElement.classList.remove('dark')
		}
	</script>
</svelte:head>

<!-- animated switch version -->
<button
	class="{dark
		? 'bg-gray-600 focus:ring-gray-400 ring-offset-gray-700'
		: 'bg-yellow-100 focus:ring-yellow-400 ring-offset-white'} relative inline-flex flex-shrink-0 h-5 w-9 border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 m-4"
	class:hidden
	type="button"
	aria-checked={dark}
	on:click={toggle}
>
	<span class="sr-only">Toggle Dark Mode</span>
	<span
		class="{dark
			? 'translate-x-0 bg-gray-300'
			: 'translate-x-4 bg-white'} pointer-events-none relative inline-block h-4 w-4 rounded-full shadow transform ring-0 transition ease-in-out duration-200"
	>
		<span
			class="{dark
				? 'opacity-100 ease-in duration-200'
				: 'opacity-0 ease-out duration-100'} absolute inset-0 h-full w-full flex items-center justify-center transition-opacity"
			aria-hidden="true"
		>
			<!-- moon icon -->
			<svg class="h-3 w-3 text-gray-500" viewBox="0 0 20 20" fill="currentColor">
				<path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" />
			</svg>
		</span>
		<span
			class="{dark
				? 'opacity-0 ease-out duration-100'
				: 'opacity-100 ease-in duration-200'} absolute inset-0 h-full w-full flex items-center justify-center transition-opacity"
			aria-hidden="true"
		>
			<!-- sun icon -->
			<svg class="h-3 w-3 text-yellow-400" viewBox="0 0 20 20" fill="currentColor">
				<path
					fill-rule="evenodd"
					d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"
					clip-rule="evenodd"
				/>
			</svg>
		</span>
	</span>
</button>

<!-- simple toggle version -->
<button
	class="{dark
		? 'focus:ring-gray-400 ring-offset-gray-700'
		: 'focus:ring-yellow-400 ring-offset-white'} border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 m-4"
	class:hidden
	on:click={toggle}
>
	<!-- moon icon -->
	<svg class="h-5 w-5 text-gray-400" class:hidden={!dark} viewBox="0 0 20 20" fill="currentColor">
		<path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" />
	</svg>
	<!-- sun icon -->
	<svg class="h-5 w-5 text-yellow-400" class:hidden={dark} viewBox="0 0 20 20" fill="currentColor">
		<path
			fill-rule="evenodd"
			d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z"
			clip-rule="evenodd"
		/>
	</svg>
</button>

<style global type="postcss">
	body {
		@apply dark:bg-slate-800;
	}
</style>
