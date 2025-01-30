<template>
	<div>
		<header class="flex flex-wrap sm:justify-start sm:flex-nowrap w-full py-3">
			<nav class="max-w-[85rem] w-full mx-auto px-4 flex flex-wrap items-center justify-end">
					<ColorModeButton />
			</nav>
		</header>
		<UContainer class="min-h-screen flex items-center">
			<UCard
				class="flex-1"
				:ui="{
					background: 'bg-gray-50 dark:bg-gray-800/50',
					ring: 'ring-1 ring-gray-300 dark:ring-gray-700',
					divide: 'divide-y divide-gray-300 dark:divide-gray-700',
					header: { base: 'font-bold' }
				}"
			>
				<template #header> Welcome to NuxtUI PhoneNumberInput component playground! </template>

				<div class="space-y-4">
					<PhoneNumberInput class="max-w-sm" v-model="phone" />

					<pre class="text-sm">{{ phone }}</pre>
				</div>
			</UCard>
		</UContainer>
	</div>
</template>

<script setup>
const phone = ref()
</script>

<style>
body {
	@apply antialiased font-sans text-gray-700 dark:text-gray-200 bg-white dark:bg-gray-900;
}
</style>
