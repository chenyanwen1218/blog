<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from "vue";

const props = defineProps<{ since: string }>();

const now = ref(Date.now());
let timer: ReturnType<typeof setInterval> | undefined;

onMounted(() => {
	timer = setInterval(() => {
		now.value = Date.now();
	}, 1000);
});

onUnmounted(() => {
	if (timer) clearInterval(timer);
});

const parts = computed(() => {
	const diff = Math.max(0, now.value - new Date(props.since).getTime());
	const s = Math.floor(diff / 1000);
	return {
		days: Math.floor(s / 86400),
		hours: Math.floor((s % 86400) / 3600),
		minutes: Math.floor((s % 3600) / 60),
		seconds: s % 60,
	};
});
</script>

<template>
	<div class="text-sm text-black/50 dark:text-white/50">
		本站已运行
		<span class="font-bold text-[var(--primary)]">{{ parts.days }}</span> 天
		{{ parts.hours }} 小时 {{ parts.minutes }} 分 {{ parts.seconds }} 秒
	</div>
</template>
