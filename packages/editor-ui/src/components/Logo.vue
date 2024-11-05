<template>
	<img :src="logoPath" :class="$style.img" alt="n8n.io" />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { mapStores } from 'pinia';
import { useRootStore } from '@/stores/n8nRoot.store';
import { useUIStore } from '@/stores/ui.store';
import { useRoute } from 'vue-router';

export default defineComponent({
	setup() {
		const route = useRoute();
		return { route };
	},
	computed: {
		...mapStores(useRootStore, useUIStore),
		basePath(): string {
			return this.rootStore.baseUrl;
		},
		logoPath(): string {
			// Check if we're on the signin page
			const isSignInPage = this.route.path === '/signin';
			if (isSignInPage) {
				const logoName = process.env.VUE_APP_LOGO_NAME || 'halfdozen';
				const suffix = this.uiStore.appliedTheme === 'dark' ? '-dark.svg' : '.svg';
				return `${this.basePath}static/logo/channel/${logoName}${suffix}`;
			}

			return this.basePath + this.uiStore.logo;
		},
	},
});
</script>

<style lang="scss" module>
.img {
	height: 32px;
}
</style>
