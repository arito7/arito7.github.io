<script lang="ts">
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';

	const links = ['Intro', 'Toolkit', 'Projects', 'Contact'];

	let ctaBtnVisible = $state(true);
	let mobileNavOpen = $state(false);

	let introEle = $state<HTMLElement>();
	let toolEle = $state<HTMLElement>();
	let projectsEle = $state<HTMLElement>();
	let contactEle = $state<HTMLElement>();

	const onClickCta = () => {
		contactEle?.scrollIntoView({ behavior: 'smooth', block: 'start' });
	};

	const onClickMenu = () => {
		mobileNavOpen = true;
		window.document.body.style.overflow = 'hidden';
	};

	const onCloseMenu = () => {
		mobileNavOpen = false;
		window.document.body.style.overflow = 'auto';
	};

	const navLinkClick = (link: String) => {
		switch (link.toLowerCase().trim()) {
			case 'intro':
				introEle?.scrollIntoView({ behavior: 'smooth', block: 'start' });
				break;
			case 'toolkit':
				toolEle?.scrollIntoView({ behavior: 'smooth', block: 'start' });
				break;
			case 'projects':
				projectsEle?.scrollIntoView({ behavior: 'smooth', block: 'start' });
				break;
			case 'contact':
				contactEle?.scrollIntoView({ behavior: 'smooth', block: 'start' });
				break;
		}
		if (mobileNavOpen) {
			onCloseMenu();
		}
	};

	onMount(() => {
		const observer = new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				if (entry.isIntersecting) {
					ctaBtnVisible = false;
				} else {
					ctaBtnVisible = true;
				}
			});
		});
		if (contactEle) observer.observe(contactEle);
	});

	if (browser) {
		const hidden = document.querySelectorAll('.hide');
		const ob = new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				if (entry.isIntersecting) {
					switch (entry.target.tagName.toLowerCase()) {
						case 'h2':
							entry.target.classList.add('show--underline');
							break;
						default:
							entry.target.classList.add('show--slide-in');
					}
				}
			});
		});
		hidden.forEach((e) => ob.observe(e));
	}
</script>

<div class="hide--underline hide--slide-in show hide show--underline show--slide-in hidden"></div>
<header class="header header--show header--top w-full bg-transparent text-slate-200 shadow">
	<div class="text-primary-content mx-auto flex h-32 max-w-5xl items-center justify-center">
		<nav class="flex w-full items-center justify-between p-6">
			<h1 class="text-primary inline-block text-3xl font-bold">Yuji️🗾</h1>
			<ul
				class={[
					mobileNavOpen ? '!translate-x-0 !opacity-100 !blur-none' : '',
					`bg-secondary fixed top-0 left-0 z-30 
					 flex h-screen w-screen translate-x-full flex-col 
					 items-start justify-start gap-8 p-8 opacity-0 blur-md 
					 transition-all duration-300 lg:static lg:flex lg:h-auto lg:w-auto 
					 lg:translate-x-0 lg:flex-row lg:bg-transparent lg:p-0 
					 lg:text-slate-300 lg:opacity-100 lg:blur-none`
				]}
			>
				<button
					aria-label="Close"
					class={['self-end', mobileNavOpen ? 'w-auto' : 'hidden']}
					onclick={onCloseMenu}
				>
					<div class="relative mt-6 w-8">
						<div class="absolute h-1 w-7 rotate-45 rounded-md bg-slate-900"></div>
						<div class="h-1 w-7 -rotate-45 rounded-md bg-slate-900"></div>
					</div>
				</button>
				{#each links as link}
					<li class="relative">
						<button
							class="hover:text-secondary before:hover:bg-secondary font-semibold before:absolute before:-bottom-1 before:left-0 before:block before:h-1 before:w-0 before:bg-slate-300 before:transition-all before:duration-300 before:content-[''] before:hover:w-full"
							onclick={() => navLinkClick(link)}>{link}</button
						>
					</li>
				{/each}
			</ul>
			<button class="cursor-pointer lg:hidden" aria-label="Menu" onclick={onClickMenu}>
				<div class="h-6 w-6">
					<div class="mb-1 h-1 rounded bg-slate-100"></div>
					<div class="mb-1 h-1 rounded bg-slate-100"></div>
					<div class="mb-1 h-1 rounded bg-slate-100"></div>
				</div>
			</button>
		</nav>
	</div>
</header>

<main class="relative">
	<button
		onclick={onClickCta}
		class={[
			'btn btn-secondary text-secondary-content fixed right-4 bottom-8 z-20 flex drop-shadow-xl transition-all hover:scale-125',
			mobileNavOpen ? 'hidden' : 'block',
			ctaBtnVisible ? '' : 'hidden'
		]}
	>
		<img class="w-6" src="./images/contact-mail.svg" alt="" />
	</button>

	<!-- intro -->
	<section
		bind:this={introEle}
		id="intro"
		class="flex min-h-screen flex-col items-center justify-center"
	>
		<div class="hero grow">
			<div class="hero-content w-screen text-center">
				<div class="flex w-full flex-col items-center justify-center gap-12">
					<h1 class="text-secondary mt-8 text-5xl font-bold">Hello there 👋</h1>
					<div class="mockup-code w-[90%] bg-slate-700 px-4 pb-12">
						<p class="text-slate-300">
							Welcome to my portfolio! I'm a self-taught programmer with a passion for creating
							intuitive and efficient digital experiences. Over the years, I've honed my skills in
							both front-end and back-end development, as well as Android app creation. Through
							hands-on projects and continuous learning, I've gained expertise in designing seamless
							user interfaces, building robust server-side systems, and developing powerful mobile
							applications. Here, you'll find a showcase of my work—each project representing my
							dedication to problem-solving and innovation. Whether it’s crafting responsive
							websites, optimizing back-end architectures, or building dynamic Android apps, I
							approach every challenge with enthusiasm and precision. Feel free to explore my
							projects, and let me know if you’d like to collaborate or learn more!
						</p>
					</div>
					<button onclick={onClickCta} class="btn btn-primary">Contact Me</button>
				</div>
			</div>
		</div>
		<div class="mx-auto flex max-w-5xl grow flex-col items-center justify-center p-9">
			{@render next_section_arrow(toolEle)}
		</div>
	</section>

	<!-- tool -->
	<section bind:this={toolEle} class="tools flex min-h-screen flex-col items-center">
		<div class="flex max-w-5xl flex-col items-center justify-center gap-8 p-9">
			<div class="card bg-base-300 self-end">
				<div class="card-body">
					<h2
						class="hide hide--underline text-secondary before:bg-secondary inline-block w-fit self-end text-3xl font-semibold md:text-4xl"
					>
						<span class="block text-3xl">Some tools I have experience in🔧</span>
					</h2>
					<ul
						class="mt-4 flex flex-col items-end justify-end text-end text-xs text-slate-400 *:mt-2"
					>
						<li class="">Languages</li>
						<li>Technologies/Frameworks</li>
						<li>Databases</li>
						<li>Libraries</li>
					</ul>
				</div>
			</div>
			<div class="tooltable w-full">
				<!-- language table -->
				<!-- name of each tab group should be unique -->
				<div class="tabs tabs-lift">
					<input type="radio" name="my_tabs_3" class="tab" aria-label="Languages" />
					<div class="tab-content bg-base-100 border-base-300 p-6">
						<table class="tooltable table-zebra table">
							<tbody>
								<tr>
									<td>Javascript | Typescript | JSX | TSX</td>
									<td class="flex flex-row">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											x="0px"
											y="0px"
											width="100"
											height="100"
											viewBox="0 0 48 48"
											class="h-8 w-8"
										>
											<path fill="#ffd600" d="M6,42V6h36v36H6z"></path><path
												fill="none"
												stroke="#000001"
												stroke-miterlimit="10"
												stroke-width="3.3"
												d="M23.783,22.352v9.819 c0,3.764-4.38,4.022-6.283,0.802"
											></path><path
												fill="none"
												stroke="#000001"
												stroke-miterlimit="10"
												stroke-width="3.3"
												d="M34.69,25.343 c-1.739-2.727-5.674-2.345-5.84,0.558c-0.214,3.757,6.768,2.938,6.247,7.107c-0.365,2.92-4.874,3.858-7.193-0.065"
											></path>
										</svg>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											x="0px"
											y="0px"
											width="100"
											height="100"
											viewBox="0 0 48 48"
											class="h-8 w-8"
										>
											<linearGradient
												id="O2zipXlwzZyOse8_3L2yya_wpZmKzk11AzJ_gr1"
												x1="15.189"
												x2="32.276"
												y1="-.208"
												y2="46.737"
												gradientUnits="userSpaceOnUse"
												><stop offset="0" stop-color="#2aa4f4"></stop><stop
													offset="1"
													stop-color="#007ad9"
												></stop></linearGradient
											><rect
												width="36"
												height="36"
												x="6"
												y="6"
												fill="url(#O2zipXlwzZyOse8_3L2yya_wpZmKzk11AzJ_gr1)"
											></rect><polygon
												fill="#fff"
												points="27.49,22 14.227,22 14.227,25.264 18.984,25.264 18.984,40 22.753,40 22.753,25.264 27.49,25.264"
											></polygon><path
												fill="#fff"
												d="M39.194,26.084c0,0-1.787-1.192-3.807-1.192s-2.747,0.96-2.747,1.986	c0,2.648,7.381,2.383,7.381,7.712c0,8.209-11.254,4.568-11.254,4.568V35.22c0,0,2.152,1.622,4.733,1.622s2.483-1.688,2.483-1.92	c0-2.449-7.315-2.449-7.315-7.878c0-7.381,10.658-4.469,10.658-4.469L39.194,26.084z"
											></path>
										</svg>
									</td>
								</tr>
								<tr>
									<td>HTML | CSS</td>
									<td class="flex flex-row justify-end">
										<svg
											xmlns="http://www.w3.org/2000/svg"
											x="0px"
											y="0px"
											width="100"
											height="100"
											viewBox="0 0 48 48"
											class="h-8 w-8"
										>
											<path fill="#E65100" d="M41,5H7l3,34l14,4l14-4L41,5L41,5z"></path><path
												fill="#FF6D00"
												d="M24 8L24 39.9 35.2 36.7 37.7 8z"
											></path><path
												fill="#FFF"
												d="M24,25v-4h8.6l-0.7,11.5L24,35.1v-4.2l4.1-1.4l0.3-4.5H24z M32.9,17l0.3-4H24v4H32.9z"
											></path><path
												fill="#EEE"
												d="M24,30.9v4.2l-7.9-2.6L15.7,27h4l0.2,2.5L24,30.9z M19.1,17H24v-4h-9.1l0.7,12H24v-4h-4.6L19.1,17z"
											></path>
										</svg>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											x="0px"
											y="0px"
											width="100"
											height="100"
											viewBox="0 0 48 48"
											class="h-8 w-8"
										>
											<path fill="#0277BD" d="M41,5H7l3,34l14,4l14-4L41,5L41,5z"></path><path
												fill="#039BE5"
												d="M24 8L24 39.9 35.2 36.7 37.7 8z"
											></path><path
												fill="#FFF"
												d="M33.1 13L24 13 24 17 28.9 17 28.6 21 24 21 24 25 28.4 25 28.1 29.5 24 30.9 24 35.1 31.9 32.5 32.6 21 32.6 21z"
											></path><path
												fill="#EEE"
												d="M24,13v4h-8.9l-0.3-4H24z M19.4,21l0.2,4H24v-4H19.4z M19.8,27h-4l0.3,5.5l7.9,2.6v-4.2l-4.1-1.4L19.8,27z"
											></path>
										</svg>
									</td>
								</tr>
								<tr>
									<td>Python</td>
									<td
										><svg
											xmlns="http://www.w3.org/2000/svg"
											x="0px"
											y="0px"
											width="100"
											height="100"
											viewBox="0 0 48 48"
											class="h-8 w-8"
										>
											<path
												fill="#0277BD"
												d="M24.047,5c-1.555,0.005-2.633,0.142-3.936,0.367c-3.848,0.67-4.549,2.077-4.549,4.67V14h9v2H15.22h-4.35c-2.636,0-4.943,1.242-5.674,4.219c-0.826,3.417-0.863,5.557,0,9.125C5.851,32.005,7.294,34,9.931,34h3.632v-5.104c0-2.966,2.686-5.896,5.764-5.896h7.236c2.523,0,5-1.862,5-4.377v-8.586c0-2.439-1.759-4.263-4.218-4.672C27.406,5.359,25.589,4.994,24.047,5z M19.063,9c0.821,0,1.5,0.677,1.5,1.502c0,0.833-0.679,1.498-1.5,1.498c-0.837,0-1.5-0.664-1.5-1.498C17.563,9.68,18.226,9,19.063,9z"
											></path><path
												fill="#FFC107"
												d="M23.078,43c1.555-0.005,2.633-0.142,3.936-0.367c3.848-0.67,4.549-2.077,4.549-4.67V34h-9v-2h9.343h4.35c2.636,0,4.943-1.242,5.674-4.219c0.826-3.417,0.863-5.557,0-9.125C41.274,15.995,39.831,14,37.194,14h-3.632v5.104c0,2.966-2.686,5.896-5.764,5.896h-7.236c-2.523,0-5,1.862-5,4.377v8.586c0,2.439,1.759,4.263,4.218,4.672C19.719,42.641,21.536,43.006,23.078,43z M28.063,39c-0.821,0-1.5-0.677-1.5-1.502c0-0.833,0.679-1.498,1.5-1.498c0.837,0,1.5,0.664,1.5,1.498C29.563,38.32,28.899,39,28.063,39z"
											></path>
										</svg></td
									>
								</tr>
								<tr>
									<td>Ruby</td>
								</tr>
								<tr>
									<td>SQL</td>
								</tr>
							</tbody>
						</table>
					</div>

					<input
						type="radio"
						name="my_tabs_3"
						class="tab"
						aria-label="Technologies/Frameworks"
						checked="checked"
					/>
					<div class="tab-content bg-base-100 border-base-300 p-6">
						<table class="table-zebra table">
							<tbody>
								<tr><td>Next.js</td></tr>
								<tr><td>React.js</td></tr>
								<tr><td>Express.js</td></tr>
								<tr><td>React Native</td></tr>
								<tr><td>Node/Deno</td></tr>
								<tr><td>Expo</td></tr>
								<tr><td>Docker</td></tr>
								<tr><td>Google Cloud</td></tr>
								<tr><td>Supabase</td></tr>
								<tr><td>Appwrite</td></tr>
								<tr><td>Supabase</td></tr>
								<tr><td>Ruby on Rails</td></tr>
								<tr><td>Git</td></tr>
								<tr><td>Svelte</td></tr>
								<tr><td>SvelteKit</td></tr>
								<tr><td>Github</td></tr>
								<tr><td>Linux</td></tr>
								<tr><td>Terminal</td></tr>
							</tbody>
						</table>
					</div>

					<input type="radio" name="my_tabs_3" class="tab" aria-label="Databases" />
					<div class="tab-content bg-base-100 border-base-300 p-6">
						<table class="table-zebra table">
							<thead>
								<tr>
									<th>Database</th>
									<th></th>
								</tr>
							</thead>
							<tbody>
								<tr
									><td>PostgreSQL</td><td
										><svg
											xmlns="http://www.w3.org/2000/svg"
											x="0px"
											y="0px"
											width="100"
											height="100"
											viewBox="0 0 48 48"
											class="h-8 w-8"
										>
											<path
												fill="#fff"
												d="M44.083,29.79c-0.183-0.829-0.935-1.796-2.452-1.796c-0.31,0-0.649,0.039-1.035,0.119c-0.708,0.146-1.311,0.217-1.842,0.241c4.133-7.04,6.816-16.819,4.159-20.214c-3.501-4.473-8.214-5.141-10.711-5.141L31.967,3c-0.929,0.015-1.893,0.129-2.863,0.339l-3.583,0.774C25.033,4.052,24.536,4.009,24.018,4l-0.03,0l-0.016,0l-0.152-0.001c-1.593,0-3.046,0.338-4.341,0.973l-1.251-0.493c-1.72-0.678-4.308-1.485-6.868-1.485c-0.144,0-0.287,0.003-0.431,0.008C8.407,3.093,6.241,4.05,4.664,5.769C2.696,7.915,1.8,11.054,2.003,15.1C2.013,15.309,4.461,36,11.4,36h0.025l0.064-0.001c0.901-0.022,1.76-0.384,2.563-1.077c0.613,0.46,1.406,0.732,2.145,0.84c0.488,0.115,1.366,0.278,2.418,0.278c1.284,0,2.442-0.263,3.44-0.738c-0.001,0.88-0.006,1.994-0.016,3.418l-0.001,0.075l0.005,0.075c0.097,1.419,0.342,2.698,0.711,3.701c1.051,2.859,2.866,4.434,5.111,4.434c0.093,0,0.188-0.003,0.284-0.009c1.846-0.114,3.717-1.151,5.004-2.772c1.393-1.755,1.715-3.607,1.839-5.026L35,39.111v-0.088v-4.079l0.103,0.01l0.436,0.038l0.042,0.004l0.042,0.002c0.124,0.006,0.252,0.008,0.381,0.008c1.507,0,3.362-0.391,4.616-0.974C41.819,33.476,44.559,31.948,44.083,29.79z"
											></path><path
												fill="#0277bd"
												d="M33,34c0-0.205,0.012-0.376,0.018-0.565C33.008,33.184,33,33,33,33s0.012-0.009,0.032-0.022c0.149-2.673,0.886-3.703,1.675-4.29c-0.11-0.153-0.237-0.318-0.356-0.475c-0.333-0.437-0.748-0.979-1.192-1.674l-0.082-0.158c-0.067-0.164-0.229-0.447-0.435-0.819c-1.183-2.14-3.645-6.592-1.96-9.404c0.738-1.232,2.122-1.942,4.121-2.117C33.986,11.718,30.925,6.115,23.985,6c-0.002,0-0.004,0-0.006,0c-6.041-0.098-8.026,5.392-8.672,8.672c0.89-0.377,1.906-0.606,2.836-0.606c0.014,0,0.029,0,0.043,0c2.29,0.017,3.865,1.239,4.323,3.354c0.335,1.552,0.496,2.91,0.492,4.153c-0.01,2.719-0.558,4.149-1.042,5.411l-0.154,0.408c-0.124,0.334-0.255,0.645-0.379,0.937c-0.126,0.298-0.237,0.563-0.318,0.802c0.484,0.11,0.864,0.265,1.125,0.38l0.151,0.066c0.047,0.02,0.094,0.043,0.137,0.069c0.848,0.516,1.376,1.309,1.489,2.233c0.061,0.498,0.051,3.893,0.03,6.855c0.087,1.285,0.305,2.364,0.593,3.146c0.409,1.114,1.431,3.241,3.394,3.119c1.37-0.085,2.687-0.919,3.561-2.019c0.938-1.181,1.284-2.487,1.414-3.958V34z"
											></path><path
												fill="#0277bd"
												d="M15.114 28.917c-1.613-1.683-2.399-3.947-2.104-6.056.285-2.035.124-4.027.037-5.098-.029-.357-.048-.623-.047-.77 0-.008.002-.015.003-.023 0-.004-.002-.007-.002-.011.121-3.021 1.286-7.787 4.493-10.62C15.932 5.724 13.388 4.913 11 5 7.258 5.136 3.636 7.724 4 15c.137 2.73 3.222 19.103 7.44 19 .603-.015 1.229-.402 1.872-1.176 1.017-1.223 2.005-2.332 2.708-3.104C15.705 29.481 15.401 29.217 15.114 28.917zM37.023 14.731c.015.154.002.286-.022.408.031.92-.068 1.813-.169 2.677-.074.636-.15 1.293-.171 1.952-.021.645.07 1.282.166 1.956.225 1.578.459 3.359-.765 5.437.225.296.423.571.581.837 4.61-7.475 6.468-16.361 4.695-18.626C38.655 5.944 34.941 4.952 31.999 5c-.921.015-1.758.139-2.473.294C34.602 7.754 36.863 13.026 37.023 14.731zM41 30.071c-2.665.55-3.947.257-4.569-.126-.1.072-.2.133-.293.19-.372.225-.961.583-1.105 2.782.083.016.156.025.246.044L35.714 33c1.32.06 3.049-.31 4.063-.781C41.962 31.205 43.153 29.627 41 30.071zM22.023 32.119c-.037-.298-.198-.539-.492-.732l-.108-.047C21.062 31.181 20.653 31 20 31h-.004c-.127.01-.253.019-.38.019-.052 0-.103-.007-.155-.009-.474.365-1.148.647-2.816.99-2.98.759-1.221 1.655-.078 1.794 1.106.277 3.735.614 5.481-.809C22.043 32.537 22.035 32.229 22.023 32.119z"
											></path><path
												fill="#0277bd"
												d="M20.681 18.501c-.292.302-.753.566-1.262.484-.828-.134-1.463-1.133-1.417-1.508h0c.044-.374.751-.569 1.578-.435.287.047.548.128.768.228-.32-.688-.899-1.085-1.782-1.182-1.565-.174-3.226.644-3.56 1.097.007.11.02.251.033.417.093 1.147.265 3.284-.05 5.537-.208 1.485.393 3.169 1.567 4.395.757.79 1.641 1.29 2.513 1.438.111-.478.309-.944.513-1.425.113-.265.233-.547.346-.852l.162-.427c.443-1.155.9-2.35.909-4.703C21.003 20.66 20.892 19.627 20.681 18.501zM34.847 22.007c-.104-.729-.211-1.484-.185-2.303.023-.742.105-1.442.184-2.119.062-.533.11-1.045.138-1.55-1.289.107-2.145.479-2.551 1.108.168-.057.358-.102.568-.129.892-.116 1.543.141 1.618.637.055.363-.253.705-.388.836-.277.269-.626.442-.981.488-.064.008-.129.012-.192.012-.353 0-.69-.121-.949-.3.112 1.973 1.567 4.612 2.283 5.907.153.277.271.498.369.688C35.154 24.163 35.009 23.143 34.847 22.007z"
											></path>
										</svg></td
									></tr
								>
								<tr><td>Sqlite</td></tr>
								<tr><td>Firebase</td></tr>
								<tr
									><td>MySQL</td>
									<td>
										<svg
											xmlns="http://www.w3.org/2000/svg"
											x="0px"
											y="0px"
											width="100"
											height="100"
											viewBox="0 0 48 48"
											class="h-8 w-8"
										>
											<path
												fill="#00796b"
												d="M0.002,35.041h1.92v-7.085l2.667,6.057c0.329,0.755,0.779,1.022,1.662,1.022 s1.315-0.267,1.644-1.022l2.667-5.902v6.93h1.92v-7.258c0-0.697-0.277-1.035-0.849-1.209c-1.367-0.43-2.285-0.059-2.7,0.872 l-2.735,6.16l-2.649-6.16c-0.398-0.93-1.332-1.302-2.7-0.872C0.277,26.748,0,27.085,0,27.782v7.258H0.002z"
											></path><path
												fill="#00796b"
												d="M13.441,29.281h1.92v4.055c-0.015,0.2,0.064,0.731,0.99,0.745c0.472,0.008,2.821,0,2.85,0v-4.8h1.92 c0.008,0,0,5.968,0,5.993c0.01,1.472-1.828,1.662-2.673,1.687h-5.006v-0.96c0.01,0,4.787,0.001,4.801,0 c1.088-0.115,0.959-0.714,0.959-0.896v-0.064H16.19c-1.67-0.015-2.735-0.751-2.747-1.59C13.441,33.373,13.479,29.317,13.441,29.281 z"
											></path><path
												fill="#f57f17"
												d="M22.081,35.041h4.807c0.63,0,1.242-0.132,1.728-0.36c0.81-0.372,1.144-0.875,1.144-1.536v-1.368 c0-1.476-1.83-1.536-2.88-1.536h-1.92c-0.755,0-0.87-0.456-0.96-0.96v-0.96c0.09-0.384,0.258-0.9,0.923-0.96 c0.773,0,4.836,0,4.836,0v-0.96h-4.566c-0.755,0-3.114,0.09-3.114,1.92v1.187c0,0.84,0.738,1.524,2.34,1.692 c0.18,0.012,0.36,0.024,0.539,0.024c0,0,1.866-0.036,1.92-0.024c1.08,0,0.96,0.84,0.96,0.96v0.96c0,0.132-0.03,0.96-0.971,0.96 c-0.072,0-4.789,0-4.789,0V35.041z"
											></path><path
												fill="#f57f17"
												d="M40.32,33.08c0,1.159,0.655,1.809,2.392,1.939c0.162,0.011,0.325,0.021,0.488,0.021H48v-0.96h-4.435 c-0.991,0-1.325-0.416-1.325-1.011v-6.669h-1.92V33.08z"
											></path><path
												fill="#f57f17"
												d="M30.704,33.121v-4.8c0-1.02,0.5-1.724,1.916-1.92h0.672h3.447h0.525 c1.416,0.196,2.08,0.899,2.08,1.92v4.782c0,0.827-0.215,1.271-0.916,1.559L39.916,36h-2.16l-1.07-0.96h-1.257l-2.136,0.012 c-0.309,0-0.635-0.043-0.993-0.141C31.226,34.618,30.704,34.054,30.704,33.121z M32.624,33.121c0.098,0.467,0.473,0.96,1.14,0.96 h1.864l-1.068-0.96h2.175l0.519,0.482c0,0,0.186-0.152,0.186-0.482c0-0.33-0.016-4.8-0.016-4.8c-0.098-0.434-0.538-0.96-1.188-0.96 h-2.471c-0.749,0-1.14,0.548-1.14,1.058L32.624,33.121L32.624,33.121z"
											></path><path
												fill="#00796b"
												d="M46.199,25.389c-1.031-0.028-1.818,0.068-2.491,0.351c-0.191,0.081-0.496,0.083-0.528,0.323 c0.105,0.11,0.121,0.275,0.205,0.41c0.16,0.26,0.432,0.609,0.674,0.791c0.265,0.2,0.538,0.414,0.821,0.587 c0.504,0.307,1.067,0.483,1.553,0.791c0.286,0.181,0.57,0.411,0.85,0.615c0.138,0.102,0.23,0.259,0.41,0.323 c0-0.01,0-0.019,0-0.029c-0.094-0.12-0.119-0.285-0.205-0.411c-0.127-0.127-0.254-0.254-0.381-0.381 c-0.372-0.494-0.846-0.929-1.348-1.289c-0.401-0.288-1.298-0.677-1.466-1.143c-0.01-0.01-0.019-0.019-0.03-0.03 c0.284-0.032,0.617-0.135,0.879-0.205c0.441-0.118,0.834-0.087,1.289-0.205c0.205-0.059,0.41-0.117,0.615-0.176 c0-0.039,0-0.078,0-0.117c-0.23-0.236-0.395-0.548-0.645-0.762c-0.657-0.559-1.373-1.117-2.11-1.583 c-0.409-0.258-0.915-0.426-1.348-0.645c-0.146-0.074-0.402-0.112-0.498-0.234c-0.228-0.29-0.351-0.659-0.527-0.996 c-0.368-0.708-0.73-1.482-1.055-2.227c-0.223-0.508-0.368-1.01-0.645-1.466c-1.331-2.188-2.764-3.509-4.982-4.807 c-0.472-0.276-1.041-0.385-1.642-0.528c-0.323-0.019-0.645-0.039-0.968-0.059c-0.197-0.083-0.401-0.323-0.587-0.44 c-0.735-0.465-2.621-1.475-3.165-0.147c-0.344,0.838,0.514,1.656,0.821,2.081c0.215,0.298,0.491,0.632,0.645,0.968 c0.101,0.22,0.119,0.441,0.205,0.674c0.213,0.574,0.55,1.228,0.826,1.759c0.139,0.269,0.293,0.551,0.469,0.791 c0.108,0.147,0.293,0.212,0.323,0.44c-0.181,0.253-0.191,0.646-0.293,0.968c-0.458,1.445-0.285,3.24,0.381,4.308 c0.204,0.328,0.686,1.032,1.348,0.762c0.579-0.236,0.45-0.967,0.615-1.612c0.037-0.146,0.014-0.253,0.088-0.351 c0,0.01,0,0.019,0,0.03c0.176,0.351,0.351,0.704,0.528,1.055c0.391,0.629,1.084,1.286,1.67,1.73 c0.304,0.23,0.544,0.628,0.938,0.762c0-0.01,0-0.019,0-0.03c-0.01,0-0.019,0-0.03,0c-0.076-0.119-0.196-0.168-0.293-0.264 c-0.229-0.225-0.485-0.504-0.674-0.762c-0.534-0.725-1.006-1.519-1.436-2.345c-0.205-0.395-0.384-0.829-0.557-1.231 c-0.067-0.155-0.066-0.389-0.205-0.469c-0.19,0.294-0.468,0.532-0.615,0.879c-0.234,0.555-0.265,1.233-0.351,1.934 c-0.052,0.018-0.029,0.006-0.059,0.029c-0.408-0.099-0.552-0.518-0.704-0.879c-0.384-0.912-0.455-2.38-0.117-3.429 c0.087-0.272,0.482-1.127,0.323-1.378c-0.076-0.251-0.328-0.396-0.468-0.587c-0.175-0.236-0.348-0.548-0.469-0.821 c-0.314-0.711-0.612-1.538-0.943-2.257c-0.158-0.344-0.425-0.691-0.645-0.996c-0.243-0.338-0.516-0.587-0.704-0.996 c-0.067-0.145-0.158-0.378-0.059-0.528c0.032-0.101,0.076-0.143,0.176-0.176c0.17-0.132,0.643,0.043,0.821,0.117 c0.47,0.195,0.862,0.381,1.26,0.645c0.191,0.127,0.384,0.372,0.615,0.44c0.088,0,0.176,0,0.264,0 c0.413,0.095,0.875,0.03,1.26,0.147c0.682,0.207,1.292,0.529,1.846,0.879c1.69,1.067,3.071,2.585,4.016,4.397 c0.152,0.292,0.218,0.57,0.351,0.879c0.27,0.624,0.611,1.266,0.879,1.876c0.268,0.609,0.53,1.223,0.909,1.73 c0.2,0.266,0.97,0.409,1.319,0.557c0.245,0.104,0.647,0.211,0.879,0.351c0.444,0.268,0.874,0.587,1.289,0.879 C45.528,24.803,46.167,25.124,46.199,25.389z"
											></path><path
												fill="#00796b"
												d="M33.098,14.223c-0.215-0.004-0.367,0.023-0.528,0.059c0,0.01,0,0.019,0,0.03c0.01,0,0.019,0,0.03,0 c0.103,0.21,0.283,0.347,0.41,0.528c0.098,0.205,0.195,0.41,0.293,0.615c0.01-0.01,0.019-0.019,0.029-0.029 c0.181-0.128,0.265-0.332,0.264-0.645c-0.073-0.077-0.084-0.173-0.147-0.264C33.365,14.394,33.203,14.325,33.098,14.223z"
											></path>
										</svg>
									</td></tr
								>
								<tr><td>MongoDB</td></tr>
							</tbody>
						</table>
					</div>

					<input type="radio" name="my_tabs_3" class="tab" aria-label="Libraries" />
					<div class="tab-content bg-base-100 border-base-300 p-6">
						<input type="radio" name="my_tabs_3" class="tab" aria-label="Tab 3" />
						<div class="tab-content bg-base-100 border-base-300 p-6"></div>
					</div>
				</div>
				{@render next_section_arrow(projectsEle)}
			</div>
		</div>
	</section>

	<!-- projects -->
	<section bind:this={projectsEle} id="projects" class="min-h-screen w-full">
		<div class="mx-auto flex max-w-5xl flex-col items-center gap-8 px-4 py-9">
			<h2 class="hide hide--underline text-secondary before:bg-primary text-4xl font-semibold">
				Projects
			</h2>
			{@render project(
				'SDSGC App',
				'/images/sdsgc.png',
				'An app for managing game characters for the mobile game Seven Deadly Sins: Grand Cross.',
				[
					'Voting system for users to rank certain characters.',
					'Runtime updates for new characters.',
					'️User data backups to Firestore',
					'Data stored in Google Spreadsheets'
				],
				50,
				2
			)}

			{@render project(
				'OSRS App',
				'/images/osrs.png',
				'An app to track real time prices for an in-game marketplace for the game Old School Runescape.',
				[
					'Hits the publicly available game API for realtime data.',
					'Graphs the data to selected time intervals.'
				],
				10,
				1
			)}

			{@render next_section_arrow(contactEle)}
		</div>
	</section>

	<section
		bind:this={contactEle}
		id="contact"
		class="bg-secondary flex min-h-screen w-full flex-col items-center justify-center"
	>
		{@render contact()}
	</section>
</main>

{@render footer()}

{#snippet next_section_arrow(toSection: HTMLElement)}
	<div class="flex w-full justify-center">
		<button
			aria-label="Scroll To Contacts"
			class="relative block h-12 w-12"
			onclick={() => {
				toSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
			}}
		>
			<svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"
				><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g
					id="SVGRepo_tracerCarrier"
					stroke-linecap="round"
					stroke-linejoin="round"
				></g><g id="SVGRepo_iconCarrier">
					<path
						d="M5.70711 9.71069C5.31658 10.1012 5.31658 10.7344 5.70711 11.1249L10.5993 16.0123C11.3805 16.7927 12.6463 16.7924 13.4271 16.0117L18.3174 11.1213C18.708 10.7308 18.708 10.0976 18.3174 9.70708C17.9269 9.31655 17.2937 9.31655 16.9032 9.70708L12.7176 13.8927C12.3271 14.2833 11.6939 14.2832 11.3034 13.8927L7.12132 9.71069C6.7308 9.32016 6.09763 9.32016 5.70711 9.71069Z"
						fill="#fff"
						class="fill-secondary"
					></path>
				</g></svg
			>
		</button>
	</div>
{/snippet}

{#snippet project(
	title: string,
	img_path: string,
	description: string,
	features: Array<string>,
	downloads: number,
	mau: number
)}
	<div class="card card-side bg-gray-900 drop-shadow-lg">
		<div class="card-body gap-8">
			<div class="flex flex-col gap-8 md:flex-row">
				<div class="avatar">
					<div class="mask mask-hexagon mx-auto w-60">
						<img class="object-cover" src={img_path} alt={`${title} Image`} />
					</div>
				</div>
				<div class="flex flex-col gap-4">
					<h3 class="card-title text-primary mx-4 text-3xl">{title}</h3>
					<p style="">
						{description}
					</p>
					<h3>✨<u><b>Features</b></u>✨</h3>
					<ul style="">
						{#each features as feature}
							<li>♦️ {feature}</li>
						{/each}
					</ul>
					<div class="stats shadow">
						<div class="stat bg-base-300">
							<div class="stat-title">Downloads</div>
							<div class="stat-value text-secondary">{downloads}k</div>
							<div class="stat-desc">MAU: ~{mau}k</div>
						</div>
					</div>
				</div>
			</div>
			<table class="table-zebra table">
				<tbody>
					<tr>
						<th>Platform</th>
						<td>Android<img class="inline-block h-8 w-8" src="/images/android.svg" alt="" /></td>
					</tr>
					<tr>
						<th>Language</th>
						<td>React Native <img src="/images/react.svg" class="inline-block h-8 w-8" alt="" /></td
						>
					</tr>
					<tr>
						<th>Framework</th>
						<td>Expo <img src="/images/expo.svg" class="inline-block h-8 w-8" alt="" /></td>
					</tr>
					<tr>
						<th class="">Backend</th>
						<td
							>Supabase
							<svg
								xmlns="http://www.w3.org/2000/svg"
								x="0px"
								class="inline-block h-8 w-8"
								y="0px"
								width="100"
								height="100"
								viewBox="0 0 48 48"
							>
								<g id="Ð¡Ð»Ð¾Ð¹_1"
									><linearGradient
										id="SVGID_1__sH0rW2TvYdr9_gr1"
										x1="14.073"
										x2="14.073"
										y1="8.468"
										y2="36.033"
										gradientUnits="userSpaceOnUse"
										><stop offset="0" stop-color="#7dffce"></stop><stop
											offset="1"
											stop-color="#50c08d"
										></stop></linearGradient
									><path
										fill="url(#SVGID_1__sH0rW2TvYdr9_gr1)"
										d="M24.2,30V6.3c0-1.8-2.3-2.6-3.4-1.2L4.5,25.9c-1.3,1.7-0.1,4.1,2,4.1H24.2z"
									></path><linearGradient
										id="SVGID_00000140728474547789280440000018204366184369975479__sH0rW2TvYdr9_gr2"
										x1="34.249"
										x2="34.249"
										y1="48.404"
										y2="19.425"
										gradientUnits="userSpaceOnUse"
										><stop offset="0" stop-color="#7dffce"></stop><stop
											offset="1"
											stop-color="#50c08d"
										></stop></linearGradient
									><path
										fill="url(#SVGID_00000140728474547789280440000018204366184369975479__sH0rW2TvYdr9_gr2)"
										d="M24,18.4v23.7c0,1.8,2.4,2.6,3.5,1.2 l16.4-20.7c1.3-1.7,0.1-4.1-2.1-4.1H24z"
									></path></g
								>
							</svg></td
						>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
{/snippet}

{#snippet contact()}
	<div class="mx-auto flex w-full max-w-5xl grow flex-col items-center justify-center gap-9 p-9">
		<h2 class="text-secondary-content text-4xl font-semibold before:bg-slate-800">Contact ☎</h2>
		<form
			class="flex w-full grow flex-col items-center justify-center gap-4 lg:w-1/2"
			action="https://api.web3forms.com/submit"
			method="POST"
		>
			<input type="hidden" name="access_key" value="b7d864e2-0a79-4044-ab01-60c6c87ea2b0" />
			<input type="checkbox" name="botcheck" class="hidden" style="display: none;" />
			<input
				class="input input-primary w-full"
				type="text"
				name="name"
				required
				placeholder="Name"
			/>
			<input
				class="input input-primary w-full"
				type="email"
				name="email"
				required
				placeholder="Email"
			/>
			<textarea
				class="textarea textarea-primary h-72 w-full"
				name="message"
				required
				placeholder=""
				aria-multiline="true"
			></textarea>
			<button type="submit" class="btn btn-primary self-end justify-self-end">
				<span>Send</span>
				<img class="fill-accent" src="/images/arrow.svg" width="28px" alt="" />
			</button>
		</form>
	</div>
{/snippet}

{#snippet footer()}
	<footer class="footer bg-neutral text-neutral-content p-10">
		<aside>
			<p>
				Yuji
				<br />
				Copyright &copy; 2025
			</p>
		</aside>
		<nav>
			<h6 class="footer-title">Social</h6>
			<div class="grid grid-flow-col gap-4">
				<a aria-label="Email" href="/">
					<svg
						class="hover:scale-105"
						width="24px"
						height="24px"
						viewBox="0 0 24 24"
						xmlns="http://www.w3.org/2000/svg"
						fill="#ffffff"
						stroke="#ffffff"
						><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g
							id="SVGRepo_tracerCarrier"
							stroke-linecap="round"
							stroke-linejoin="round"
						></g><g id="SVGRepo_iconCarrier">
							<defs>
								<style>
									.cls-1,
									.cls-2 {
										fill: none;
										stroke: #ffffff;
										stroke-linecap: round;
										stroke-width: 2.4;
									}
									.cls-1 {
										stroke-linejoin: bevel;
									}
									.cls-2 {
										stroke-linejoin: round;
										fill-rule: evenodd;
									}
								</style>
							</defs>
							<g id="ic-contact-mail">
								<rect class="cls-1" x="2" y="5" width="20" height="14" rx="2"></rect>
								<path class="cls-2" d="M2.58,5.59l8.23,8.22a2,2,0,0,0,2.83,0l8-8"></path>
							</g>
						</g></svg
					>
				</a>
				<a aria-label="Link to Github" href="https://www.github.com/arito7" target="_blank">
					<svg
						class="hover:scale-105"
						width="24px"
						height="24px"
						viewBox="0 0 24 24"
						fill="none"
						xmlns="http://www.w3.org/2000/svg"
						><g id="SVGRepo_bgCarrier" stroke-width="0"></g><g
							id="SVGRepo_tracerCarrier"
							stroke-linecap="round"
							stroke-linejoin="round"
						></g><g id="SVGRepo_iconCarrier">
							<path
								fill-rule="evenodd"
								clip-rule="evenodd"
								d="M20.9992 5.95846C21.0087 6.565 20.9333 7.32649 20.8658 7.8807C20.8395 8.09686 20.8037 8.27676 20.7653 8.42453C21.6227 10.01 22 11.9174 22 14C22 16.4684 20.8127 18.501 18.9638 19.8871C17.1319 21.2605 14.6606 22 12 22C9.33939 22 6.86809 21.2605 5.0362 19.8871C3.18727 18.501 2 16.4684 2 14C2 11.9174 2.37732 10.01 3.23472 8.42452C3.19631 8.27676 3.16055 8.09685 3.13422 7.8807C3.06673 7.32649 2.99133 6.565 3.00081 5.95846C3.01149 5.27506 3.10082 4.5917 3.19988 3.91379C3.24569 3.60028 3.31843 3.30547 3.65883 3.11917C4.00655 2.92886 4.37274 2.99981 4.73398 3.1021C5.95247 3.44713 7.09487 3.93108 8.16803 4.51287C9.2995 4.17287 10.5783 4 12 4C13.4217 4 14.7005 4.17287 15.832 4.51287C16.9051 3.93108 18.0475 3.44713 19.266 3.1021C19.6273 2.99981 19.9935 2.92886 20.3412 3.11917C20.6816 3.30547 20.7543 3.60028 20.8001 3.91379C20.8992 4.5917 20.9885 5.27506 20.9992 5.95846ZM20 14C20 12.3128 19.6122 10 17.5 10C16.5478 10 15.6474 10.2502 14.7474 10.5004C13.8482 10.7502 12.9495 11 12 11C11.0505 11 10.1518 10.7502 9.25263 10.5004C8.35261 10.2502 7.45216 10 6.5 10C4.39379 10 4 12.3197 4 14C4 15.7636 4.82745 17.231 6.23588 18.2869C7.66135 19.3556 9.69005 20 12 20C14.3099 20 16.3386 19.3555 17.7641 18.2869C19.1726 17.231 20 15.7636 20 14ZM10 14.5C10 15.8807 9.32843 17 8.5 17C7.67157 17 7 15.8807 7 14.5C7 13.1193 7.67157 12 8.5 12C9.32843 12 10 13.1193 10 14.5ZM15.5 17C16.3284 17 17 15.8807 17 14.5C17 13.1193 16.3284 12 15.5 12C14.6716 12 14 13.1193 14 14.5C14 15.8807 14.6716 17 15.5 17Z"
								fill="#ffffff"
							></path>
						</g></svg
					>
				</a>
			</div>
		</nav>
	</footer>
{/snippet}

<style lang="scss">
	.hide {
		&--slide-in {
			transform: translateX(-100%);
			opacity: 0;
			filter: blur(5px);
			transition: all 0.8s;
		}
		&--underline {
			position: relative;
			width: fit-content;
			&::before {
				position: absolute;
				bottom: -8px;
				left: 0;
				opacity: 0;
				transition: all 0.3s;
				border-radius: 4px;
				width: 0%;
				height: 4px;
				content: '';
			}
		}
	}

	.show {
		&--slide-in {
			transform: translateX(0);
			opacity: 1;
			filter: blur(0);
		}
		&--underline::before {
			opacity: 1 !important;
			transition-delay: 500ms;
			width: 100% !important;
		}
	}

	.tooltable table > tbody > tr > td:nth-child(2) {
		display: flex;
		flex-direction: row;
		justify-content: end;
		gap: 1rem;
	}
</style>
