<script setup lang="ts">
	import {ref, computed, onMounted} from 'vue'

	const themes = ref(['light', 'dark'])
	const currentIndex = ref(0)

	const currentTheme = computed(() => themes.value[currentIndex.value])
	const nextTheme = computed(() => themes.value[(currentIndex.value + 1) % themes.value.length])
	const displayTheme = computed(() =>
		nextTheme.value.charAt(0).toUpperCase() + nextTheme.value.slice(1) + ' Theme'
	)

	function toggle() {
		document.body.classList.remove(currentTheme.value)
		currentIndex.value = (currentIndex.value + 1) % themes.value.length
		document.body.classList.add(currentTheme.value)
	}

	function addTheme(themeName: string) {
		if (!themes.value.includes(themeName)) {
			themes.value.push(themeName)
		}
	}

	onMounted(() => {
		const prefersDark = window.matchMedia('(prefers-color-scheme: dark)')

		if (prefersDark.matches) {
			toggle()
		}
	})

	defineExpose({addTheme})
</script>

<template>
	<button @click="toggle" class="theme-button">
		{{ displayTheme }}
	</button>
</template>
