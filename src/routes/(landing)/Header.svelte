<script lang="ts">
	import Icon from '@iconify/svelte';
	import ToggleTheme from '$lib/components/utils/ToggleTheme.svelte';
	import { page } from '$app/stores';
	import { navLinks as links } from '$lib/data/links';
	let hidden = true;
</script>

<!-- NOTE: https://www.braydoncoyer.dev/blog/build-a-glassmorphic-navbar-with-tailwindcss-backdrop-filter-and-backdrop-blur -->
<div
	class="sticky left-0 top-0 z-10 backdrop-blur-md lg:sticky lg:bg-opacity-90"
>
	<header class="mx-auto max-w-7xl justify-center">
		<nav
			class="mx-auto flex max-w-7xl items-center justify-between bg-gray-600 bg-opacity-5
         p-6 backdrop-filter lg:rounded lg:border lg:border-gray-800 lg:px-8"
			aria-label="Global"
		>
			<a href="/" class="-m-1.5 p-1.5">
				<span class="sr-only">Daedalus</span>

				<span
					class="h-8 w-auto text-2xl font-black text-black dark:text-white lg:block"
					>Daedal<span class="font-black text-accent dark:text-accent">us</span
					></span
				>
			</a>
			<div class="flex lg:hidden">
				<button
					type="button"
					class="z-10 -m-2.5 inline-flex items-center justify-center rounded-md p-2.5 text-gray-500 dark:text-white lg:hidden"
					on:click={() => (hidden = !hidden)}
				>
					<span class="sr-only">Open main menu</span>
					<Icon icon="mdi:menu" />
				</button>
			</div>
			<div class="hidden lg:flex lg:gap-x-8">
				{#each links as link}
					<a
						href={link.link}
						class="text-sm font-light leading-6 text-gray-900
              hover:text-secondary active:text-primary
              dark:text-white dark:hover:text-secondary"
						class:active={$page.url.pathname === link.link}>{link.text}</a
					>
				{/each}
				<a
					href="/login"
					class="text-sm font-light leading-6 text-gray-900 hover:text-secondary
              dark:text-white dark:hover:text-secondary">Log in</a
				>
				<ToggleTheme />
			</div>
		</nav>
		<!-- Mobile menu, show/hide based on menu open state. -->
		<div class=" lg:hidden" {hidden} role="dialog" aria-modal="true">
			<!-- Background backdrop, show/hide based on slide-over state. -->
			<div
				class="fixed inset-0 z-10 cursor-auto bg-opacity-25 backdrop-blur-[2px] backdrop-filter"
				{hidden}
				on:click={() => (hidden = !hidden)}
				role="button"
				tabindex="0"
				on:keydown={(event) => {
					if (event.key === 'Enter' || event.key === ' ') {
						hidden = !hidden;
					}
				}}
			></div>
			<div
				data-navbar-container
				class="navbarBackground fixed inset-y-0 right-0 z-10 h-[100vh] w-full overflow-y-auto border border-gray-800 px-6 py-6 sm:max-w-sm sm:ring-1 sm:ring-gray-900/10"
			>
				<div class="flex items-center justify-between">
					<a href="/" class="-m-1.5 p-1.5">
						<span class="sr-only">Daedalus</span>
						<span
							class="h-8 w-auto text-2xl font-black text-white dark:text-white"
							>Daedal<span class="font-black text-accent dark:text-accent"
								>us</span
							></span
						>
					</a>
					<button
						type="button"
						class="-m-2.5 rounded-md p-2.5 text-slate-900"
						on:click={() => (hidden = !hidden)}
					>
						<span class="sr-only">Close menu</span>
						<Icon icon="mdi:close" />
					</button>
				</div>
				<div class="mt-6 flow-root">
					<div class="divide-white-700 -my-6 divide-y">
						<div class="space-y-2 py-6">
							{#each links as link}
								<a
									href={link.link}
									class="-mx-3 block rounded-lg px-3 py-1 text-center font-semibold leading-5 text-white hover:bg-gray-50 hover:text-black active:text-primary dark:text-white hover:dark:text-black"
									class:active={$page.url.pathname === link.link}
									on:click={() => (hidden = !hidden)}
									>{link.text}
								</a>
							{/each}
							<div
								class="  w-full rounded-lg px-3 py-1 hover:bg-gray-50 hover:text-black dark:text-white hover:dark:text-black"
							>
								<ToggleTheme mobile={true} />
							</div>
						</div>
						<div class="py-6">
							<a
								href="/register"
								class="-mx-3 block rounded-lg px-3 py-1 text-center text-base font-semibold leading-5 text-white hover:bg-gray-50 hover:text-black dark:text-white hover:dark:text-black"
								>Sign up</a
							>
							<a
								href="/Login"
								class="-mx-3 block rounded-lg px-3 py-1 text-center text-base font-semibold leading-5 text-white hover:bg-gray-50 hover:text-black dark:text-white hover:dark:text-black"
								>Login</a
							>
						</div>
					</div>
				</div>
			</div>
		</div>
	</header>
</div>

<style>
	[data-navbar-container].navbarBackground::before {
		content: ''; /* Required to create a pseudo-element */
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		z-index: -1;

		background-color: var(--color-primary);
		opacity: 0.5;
		filter: blur(25px);
	}
</style>
