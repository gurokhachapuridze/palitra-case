<template>
	<OnClickOutside @trigger="dropdownIsOpen = false">
		<div class="multiselect-wrapper" ref="target">
			<div
				class="multiselect-select"
				:class="{ active: dropdownIsOpen }"
				@click="toggleDropdown"
			>
				<svg
					xmlns="http://www.w3.org/2000/svg"
					xml:space="preserve"
					width="298"
					height="512"
					shape-rendering="geometricPrecision"
					text-rendering="geometricPrecision"
					image-rendering="optimizeQuality"
					fill-rule="evenodd"
					clip-rule="evenodd"
					viewBox="0 0 298 511.93"
				>
					<path
						fill-rule="nonzero"
						d="M70.77 499.85c-16.24 16.17-42.53 16.09-58.69-.15-16.17-16.25-16.09-42.54.15-58.7l185.5-185.03L12.23 70.93c-16.24-16.16-16.32-42.45-.15-58.7 16.16-16.24 42.45-16.32 58.69-.15l215.15 214.61c16.17 16.25 16.09 42.54-.15 58.7l-215 214.46z"
					/>
				</svg>
			</div>
			<div class="selection">
				<div
					class="tag"
					v-for="(tag, index) in selectedOptions"
					:key="index"
					@click="removeTag(tag)"
				>
					{{ tag }}
					<span>
						<svg
							version="1.1"
							id="Capa_1"
							xmlns="http://www.w3.org/2000/svg"
							xmlns:xlink="http://www.w3.org/1999/xlink"
							x="0px"
							y="0px"
							viewBox="0 0 490.29 490.29"
							style="enable-background: new 0 0 490.29 490.29"
							xml:space="preserve"
						>
							<g>
								<g>
									<g>
										<rect
											x="206.343"
											y="-62.678"
											transform="matrix(-0.7071 0.7071 -0.7071 -0.7071 591.6399 245.173)"
											width="77.399"
											height="615.594"
										/>

										<rect
											x="-9.144"
											y="335.976"
											transform="matrix(0.7071 -0.7071 0.7071 0.7071 -231.1054 191.4143)"
											width="249.298"
											height="77.399"
										/>

										<rect
											x="250.136"
											y="77.228"
											transform="matrix(-0.7071 0.7071 -0.7071 -0.7071 721.7715 -67.1118)"
											width="249.298"
											height="77.399"
										/>
									</g>
								</g>
							</g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
							<g></g>
						</svg>
					</span>
				</div>
				<input
					class="input-search"
					placeholder="search..."
					type="text"
					:class="{ active: dropdownIsOpen }"
					v-model="search"
				/>
			</div>
			<div
				class="options"
				@keyup="nextItem"
				:class="{ active: dropdownIsOpen }"
			>
				<ul>
					<li
						tabindex="0"
						v-for="(item, index) in optionList"
						:key="index"
						:class="{ 'active-item': currentItem === index + 1 }"
						@click="selectOption(item)"
						@keyup.enter="currentItem === index + 1 ? selectOption(item) : ''"
					>
						{{ item }}
					</li>
				</ul>
			</div>
		</div>
    
	</OnClickOutside>
</template>

<script>
	import { onMounted, ref, watch } from 'vue';
	import { OnClickOutside } from '@vueuse/components';
	export default {
		name: 'HelloWorld',
		components: {
			OnClickOutside,
		},
		props: {
			options: {
				type: Array,
				default: () => [],
			},
		},

		setup(props) {
			const target = ref();

			const value = ref(null);
			const currentItem = ref(1);

			const dropdownIsOpen = ref(null);

			const search = ref('');

			const optionList = ref(props.options);
			const selectedOptions = ref([]);
			const removeTag = (item) => {
				const index = selectedOptions.value.indexOf(item);
				selectedOptions.value.splice(index, 1);
				// optionList.value.push(item);
			};
			const selectOption = (item) => {
				selectedOptions.value.push(item);
				// const index = optionList.value.indexOf(item);
				// optionList.value.splice(index, 1);
				search.value = '';
			};
			const toggleDropdown = () => {
				dropdownIsOpen.value = !dropdownIsOpen.value;
				search.value = '';
			};

			const nextItem = (event) => {
				if (event.keyCode == 38 && currentItem.value > 1) {
					currentItem.value--;
				} else if (event.keyCode == 40 && currentItem.value < 3) {
					currentItem.value++;
				}
				event.keyCode == 13 && dropdownIsOpen.value
					? selectOption(optionList.value[currentItem.value - 1])
					: '';
			};

			onMounted(() => {
				document.addEventListener('keyup', nextItem);
			});

			watch(search, (newSearch) => {
				if (newSearch.length)
					optionList.value = optionList.value.filter((i) => i.match(newSearch));
				else optionList.value = props.options;
			});

			return {
				value,
				optionList,
				selectOption,
				selectedOptions,
				removeTag,
				toggleDropdown,
				dropdownIsOpen,
				search,
				target,
				currentItem,
			};
		},
	};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="@vueform/multiselect/themes/default.css"></style>
<style scoped>
	.multiselect-wrapper {
		position: relative;
	}
	.selection {
		width: 96%;
		display: flex;
		align-items: center;
		max-height: 60px;
		padding: 0 20px;
		height: 60px;
		background: #d5d5d5;
		border-radius: 10px;
		cursor: pointer;
	}
	.multiselect-select {
		position: absolute;
		width: 100%;
		height: 100%;
		right: 40px;
		top: 50%;
		transform: translate(20px, -50%);
		z-index: 2;
	}
	.multiselect-select.active svg {
		transform: translate(20px, -50%) rotate(270deg);
	}

	.input-search {
		z-index: 999999;
		width: 60%;
		height: 60px;
		border: unset;
		left: 0;
		opacity: 0;
		visibility: hidden;
		background: transparent;
	}
	.input-search.active {
		opacity: 1;
		visibility: visible;
		transition: 0.4s ease;
	}
	input:focus {
		outline: none;
	}
	.multiselect-select svg {
		width: 10px;
		height: 10px;
		right: 40px;
		top: 50%;
		transform: translate(20px, -50%) rotate(90deg);
		position: absolute;
		transition: 0.4s ease;
	}

	.tag {
		background: #47c647;
		border-radius: 4px;
		padding: 4px 8px;
		display: flex;
		align-items: center;
		margin-right: 10px;
		max-height: 30px;
		z-index: 3;
		position: relative;
	}
	.tag span {
		display: flex;
		align-items: center;
	}
	.tag svg {
		width: 10px;
		margin-left: 10px;
	}

	.options {
		position: absolute;
		left: 0;
		top: 75px;
		width: 100%;
		opacity: 0;
		visibility: hidden;
		z-index: 999;
		background-color: white;
		box-shadow: rgba(0, 0, 0, 0.16) 0px 10px 36px 0px,
			rgba(0, 0, 0, 0.06) 0px 0px 0px 1px;
		transition: 0.4s ease;
	}
	.options .active-item {
	}
	.options.active {
		opacity: 1;
		visibility: visible;
		transition: 0.4s ease;
	}
	.options.active:focus {
	}
	.options ul {
		display: flex;
		flex-direction: column;
	}
	.options ul li {
		cursor: pointer;
		padding: 5px;
	}
	.options ul li:hover,
	.options ul li.active-item {
		background-color: whitesmoke;
	}
	h3 {
		margin: 40px 0 0;
	}
	ul {
		list-style-type: none;
		padding: 0;
	}
	li {
		display: inline-block;
		margin: 0 10px;
	}
	a {
		color: #42b983;
	}
</style>
