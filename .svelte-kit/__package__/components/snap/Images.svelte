<script>
	import Button from '../basic_elements/Button.svelte';
	import Icon from '../basic_elements/Icon.svelte';
	import get from 'lodash/get';

	import {createEventDispatcher} from 'svelte';
	const dispatch = createEventDispatcher();

	export let images = [];

	const setCover = selectedImage => {
		images = images.map(image => {
			if (image === selectedImage) {
				image.cover = true;

				dispatch('selectCover', image);
			} else {
				image.cover = false;
			}
			return image;
		});
	};

	images = images.map(image => ({...image, mouseOver: false}));

	function handleRemoveImg(image) {
		dispatch('remove', image);
	}
</script>

{#each images as image (image.id)}
	<span
		class="images"
		on:mouseenter={() => (image.mouseOver = true)}
		on:mouseleave={() => (image.mouseOver = false)}
		role="button"
		tabindex="0"
	>
		<span class="image_action">
			<div class="action_layout">
				{#if image.mouseOver}
					<Icon
						name="trash"
						clickable={true}
						size="2rem"
						class="cursor_pointer fill_dark_grey hover_primary_purple"
						on:click={() => handleRemoveImg(image)}
						viewSize={{width: '40', height: '40'}}
					/>

					{#if get(image, 'cover', false) === true}
						<Button label="Cover" class="action_active" disabled={true} />
					{/if}

					{#if get(image, 'cover', false) === false}
						<Button
							label="Cover"
							class="action_disabled"
							on:click={() => setCover(image)}
						/>
					{/if}
				{/if}
			</div>
		</span>

		<img src={image.url} alt="img" />
	</span>
{/each}

<style>
	.images {
    position: relative;
    margin-bottom: 2rem;
    display: flex;
    flex-direction: column;
    align-items: center
}
	.image_action {
    position: absolute;
    bottom: 1rem;
    right: 1rem
}
	.action_layout {
    display: flex;
    flex-direction: row;
    justify-content: center;
    gap: 1rem
}
</style>
