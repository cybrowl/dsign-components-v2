<script>
	import {get} from 'lodash';
	import {createEventDispatcher, onMount} from 'svelte';
	const dispatch = createEventDispatcher();
	import {get_topic_by_id} from '../../utils/topics';

	import TopicSidebar from './TopicSidebar.svelte';
	import Conversation from './Conversation.svelte';

	export let project = {};
	export let selected_topic_id = '';

	const topics = get(project, 'feedback.topics[0]', []);
	const is_owner = get(project, 'is_owner', false);

	let selected_topic = get_topic_by_id(topics, selected_topic_id);

	function select_topic(event) {
		event.stopPropagation();

		selected_topic = {...selected_topic, ...event.detail.selected_topic};

		dispatch('select_topic', event.detail);
	}

	function select_file(event) {
		event.stopPropagation();

		dispatch('select_file', {file: event.detail, selected_topic});
	}

	function send_message(event) {
		event.stopPropagation();
		const {content} = event.detail;

		dispatch('send_message', {selected_topic, content});
	}

	function download_file(event) {
		event.stopPropagation();

		dispatch('download_file', event.detail);
	}

	function accept_change(event) {
		event.stopPropagation();

		dispatch('accept_change', selected_topic);
	}

	function reject_change(event) {
		event.stopPropagation();

		dispatch('reject_change', selected_topic);
	}

	function remove_topic(event) {
		event.stopPropagation();

		dispatch('remove_topic', event.detail);
	}
</script>

<div class="topic_sidebar">
	<TopicSidebar
		{topics}
		{selected_topic}
		{is_owner}
		on:remove_topic={remove_topic}
		on:select_topic={select_topic}
	/>
</div>

<div class="conversation">
	{#if topics.length > 0}
		<Conversation
			{selected_topic}
			{is_owner}
			on:send_message={send_message}
			on:accept_change={accept_change}
			on:reject_change={reject_change}
			on:select_file={select_file}
			on:download_file={download_file}
		/>
	{/if}
</div>

<style>
	.topic_sidebar {
    grid-column-start: 1;
    grid-column-end: 4;
    --tw-text-opacity: 1;
    color: rgb(235 233 255 / var(--tw-text-opacity))
}
	.conversation {
    grid-column-start: 4;
    grid-column-end: 13
}
</style>
