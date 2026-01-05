<template>
	<UApp>
		<div class="antialiased text-gray-700 dark:text-gray-200 bg-white dark:bg-gray-900">
			<header class="flex flex-wrap sm:justify-start sm:flex-nowrap w-full py-3">
				<nav class="max-w-340 w-full mx-auto px-4 flex flex-wrap items-center justify-end">
					<UColorModeButton />
				</nav>
			</header>
			<UContainer class="min-h-screen flex items-center justify-center">
				<UCard variant="subtle">
					<template #header> NuxtUI PhoneNumberInput component playground! </template>
	
					<div class="space-y-4">
						<PhoneNumberInput class="w-full" v-model="phone" />
	
						<pre class="text-sm">{{ phone }}</pre>
					</div>
				</UCard>
			</UContainer>
		</div>
	</UApp>
</template>

<script setup>
const phone = ref()
</script>
