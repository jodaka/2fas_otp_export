<script lang="ts">
	import { onMount } from 'svelte';
	import QRCode from 'qrcode';
	let files: FileList;

	type qrcodeItem = {
		name: string;
		image: string;
	};

	const LIGHT_THEME = {
		light: '#ffffff',
		dark: '#26325c'
	};

	const DARK_THEME = {
		light: '#17212b',
		dark: '#ffffff'
	};

	let colors = LIGHT_THEME;

	onMount(async () => {
		colors =
			window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches
				? DARK_THEME
				: LIGHT_THEME;
	});

	type service = {
		name: string;
		otp: {
			link: string;
		};
	};

	type twofasExport = {
		services: service[];
	};

	let data: qrcodeItem[] = [];

	const generateQrCode = (services: service[]) => {
		data = new Array(services.length).fill(null);

		services.forEach((srv, index) => {
			data[index] = {
				name: srv.name,
				image: ''
			};

			QRCode.toString(srv.otp.link, {
				type: 'svg',
				margin: 1,
				color: colors
			})
				.then((url: string) => {
					data[index].image = url;
				})
				.catch((err: Error) => {
					console.error(err);
					data[index].image = '';
				});
		});
	};

	const parseJSON = (src: Blob): Promise<twofasExport> => {
		return new Promise((resolve, reject) => {
			const reader = new FileReader();
			reader.readAsText(src);
			reader.onload = function () {
				try {
					const res = JSON.parse(reader.result as string);
					resolve(res);
				} catch (e) {
					reject(e);
				}
			};
			reader.onerror = reject;
		});
	};

	$: if (files) {
		parseJSON(files[0]).then((res: twofasExport) => {
			if (res?.services?.length) {
				generateQrCode(res.services);
			}
		});
	}
</script>

<div class="wrapper">
	<h1>2FAS export to qr code converter</h1>

	<label for="exportJson">Upload a 2FAS export:</label>
	<input bind:files name="exportJson" type="file" accept=".2fas" />

	{#if data}
		<ul class="qrcodes">
			{#each data as srv}
				<li class="qrcode">
					<h2>{srv.name}</h2>
					<div class="qrcode__img">{@html srv.image}</div>
				</li>
			{/each}
		</ul>
	{/if}
</div>

<style>
	.wrapper {
		margin: 0 auto;
		width: fit-content;
	}
</style>
