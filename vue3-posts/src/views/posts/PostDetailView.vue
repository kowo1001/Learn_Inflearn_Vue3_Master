<template>
	<div>
		<h2>{{ post.title }}</h2>
		<p>{{ post.content }}</p>
		<p class="text-muted">{{ post.createdAt }}</p>
		<hr class="my-4" />
		<div class="row g-2">
			<div class="col-auto">
				<button class="btn btn-outline-dark">이전글</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-dark">다음글</button>
			</div>
			<div class="col-auto me-auto"></div>
			<div class="col-auto">
				<button class="btn btn-outline-dark" @click="goListPage">목록</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-primary" @click="goEditPage">
					수정
				</button>
			</div>
			<div class="col-auto">
				<button class="btn btn-outline-danger" @click="remove">삭제</button>
			</div>
		</div>
		<hr class="my-4" />
		{{ $route.params }}
	</div>
</template>

<script setup>
import { useRouter } from 'vue-router';
import { getPostById } from '@/api/posts';
import { ref } from 'vue';
// import { formToJSON } from 'axios';

const props = defineProps({
	id: String,
});

// const route = useRoute();
const router = useRouter();
// const id = route.params.id;
// console.log('post:', getPostById(id));

// const fetchPost = () => {
// 	const data = getPostById(id);
// 	form.value = { ...data };
// };
// fetchPost();

/**
 * ref
 * 장점) 객체 할당 가능
 * 장점) 일관성이 있음 (일관성 유지 가능)
 * 단점) form.value.title, form.value.content
 *
 * reactive
 * 장점) form.title, form.content
 * 단점) 객체 할당 불가능
 *
 */

const post = ref({
	title: null,
	content: null,
	createdAt: null,
});

const fetchPost = async () => {
	try {
		const { data } = await getPostById(props.id);
		// post.value = { ...data }; // 어떤 데이터가 들어오는지 모름
		setPost(data);
	} catch (error) {
		console.error(error);
	}
};
const setPost = ({ title, content, createdAt }) => {
	post.value.title = title;
	post.value.content = content;
	post.value.createdAt = createdAt;
};

fetchPost();
const remove = async () => {
	try {
		if (confirm('삭제 하시겠습니까?') === false) {
			return;
		}
		await defineProps(props.id);
		router.push({ name: 'PostList' });
	} catch (error) {
		console.error(error);
	}
};

const goListPage = () => router.push({ name: 'PostList' });
const goEditPage = () =>
	router.push({ name: 'PostEdit', params: { id: props.id } });
</script>

<style lang="scss" scoped></style>
