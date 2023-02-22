<template>
	<div>
		<h2>게시글 목록</h2>
		<hr class="my-4" />
		<div class="row g-3">
			<div v-for="post in posts" :key="post.id" class="col-4">
				<PostItem
					:title="post.title"
					:content="post.content"
					:created-at="post.createdAt"
					@click="goPage(post.id)"
				></PostItem>
			</div>
		</div>
		<hr class="my-4" />
	</div>
</template>

<script setup>
import PostItem from '@/components/posts/PostItem.vue';
import PostDetailView from '@/views/posts/PostDetailView.vue';

import { getPosts } from '@/api/posts';
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const posts = ref([]);
const fetchPosts = async () => {
	// ({ data: posts.value } = await getPosts()); // 이렇게도 받을 수 있음
	try {
		const { data } = await getPosts();
		posts.value = data;
	} catch (error) {
		console.error(error);
	}

	// getPosts().then(response => {
	// 	console.log('response: ', response);
	// }).catch(error => {
	// 	console.log('error: ', error);
	// })
};
fetchPosts();
const goPage = (id) => {
	// router.push(`/posts/${id}`);
	// http://127.0.0.1:5173/posts/5?searchText=hello#world!
	router.push({
		name: 'PostDetail',
		params: {
			id,
		},
		// query: {
		// 	searchText: 'hello',
		// },
		// hash: '#world!',
	});
};
</script>

<style lang="scss" scoped></style>
