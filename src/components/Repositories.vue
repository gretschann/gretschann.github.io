<template lang="html">
	<div>
		<h3 class="font-weight-black pb-4">Saved repositorys</h3>
		<div v-for="repo in repositories">
			<p class="font-weight-black">
				<a :href="repo.html_url">
					{{repo.full_name}}
				</a>
			</p>
		</div>
	</div>
</template>

<script>
export default {
	props: ['repositories'],
}
</script>
