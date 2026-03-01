<script lang="ts">
	import { onMount } from 'svelte';
	import { Droplets, Rocket, Shield, Globe, GitBranch, Activity, Terminal, ChevronRight, Check, Server, Key } from 'lucide-svelte';
	import AuthModal from '$lib/components/ui/AuthModal.svelte';
	import { currentUser } from '$lib/stores/auth';

	// ── Scroll reveal ─────────────────────────────────────────
	function initScrollReveal() {
		const els = document.querySelectorAll('.reveal');
		const obs = new IntersectionObserver((entries) => {
			entries.forEach((e) => {
				if (e.isIntersecting) {
					(e.target as HTMLElement).classList.add('revealed');
					obs.unobserve(e.target);
				}
			});
		}, { threshold: 0.12 });
		els.forEach(el => obs.observe(el));
	}

	// ── Smooth scroll ─────────────────────────────────────────
	function smoothTo(id: string) {
		return (e: MouseEvent) => {
			e.preventDefault();
			document.getElementById(id)?.scrollIntoView({ behavior: 'smooth', block: 'start' });
		};
	}

	onMount(() => initScrollReveal());

	// ── Auth Modal State ──────────────────────────────────────
	let showAuthModal = $state(false);
	let authMode = $state<'login' | 'register'>('login');

	function openAuth(mode: 'login' | 'register') {
		authMode = mode;
		showAuthModal = true;
	}
</script>

<svelte:head>
	<title>SyncShip — Deploy Smarter. Ship Faster.</title>
	<meta name="description" content="The self-hosted deployment platform for agencies. Git-push to deploy. Your server. Your control." />
</svelte:head>

<!-- ─── NAVBAR ────────────────────────────────────────────────── -->
<header class="fixed top-0 inset-x-0 z-50 border-b border-gray-200/80 bg-white/70 backdrop-blur-xl">
	<div class="mx-auto max-w-7xl px-6 lg:px-8">
		<div class="flex h-16 items-center justify-between">
			<a href="/" class="flex items-center gap-x-2.5">
				<div class="rounded-lg bg-gray-900 p-1.5 shadow-sm">
					<Droplets class="h-4 w-4 text-white" />
				</div>
				<span class="text-lg font-bold tracking-tight text-gray-900">SyncShip</span>
			</a>

			<nav class="hidden md:flex items-center gap-8">
				<a href="#features"     onclick={smoothTo('features')}     class="text-sm font-medium text-gray-500 hover:text-gray-900 transition-colors">Features</a>
				<a href="#how-it-works" onclick={smoothTo('how-it-works')} class="text-sm font-medium text-gray-500 hover:text-gray-900 transition-colors">How it works</a>
				<a href="#pricing"      onclick={smoothTo('pricing')}      class="text-sm font-medium text-gray-500 hover:text-gray-900 transition-colors">Pricing</a>
			</nav>

			<div class="flex items-center gap-3">
				<button 
					onclick={() => openAuth('login')} 
					class="hidden sm:block text-sm font-bold text-gray-500 hover:text-gray-900 transition-colors cursor-pointer"
				>
					Sign in
				</button>
				<button 
					onclick={() => openAuth('register')} 
					class="rounded-lg bg-gray-900 px-4 py-2 text-sm font-bold text-white hover:bg-gray-700 transition-colors shadow-sm cursor-pointer"
				>
					Get Started Free
				</button>
			</div>
		</div>
	</div>
</header>

<!-- ─── HERO ──────────────────────────────────────────────────── -->
<section class="relative min-h-screen flex items-center justify-center overflow-hidden pt-32 pb-24">
	<div class="relative z-10 mx-auto max-w-5xl px-6 text-center">

		<h1 class="reveal fade-up delay-100 text-5xl sm:text-6xl lg:text-7xl font-black text-gray-900 tracking-tight leading-[1.05]">
			Own Your Server.
			<br />
			<span class="relative inline-block mt-2 text-indigo-600">
				<span class="relative z-10">Deploy Like Pro.</span>
				<span class="absolute inset-x-0 bottom-1 h-4 bg-indigo-100/70 -rotate-1 rounded"></span>
			</span>
		</h1>

		<p class="reveal fade-up delay-200 mt-7 text-lg sm:text-xl text-gray-500 max-w-2xl mx-auto leading-relaxed font-medium">
			Stop fighting "Serverless" limits. Get the one-click experience on <strong class="text-gray-900">your own hardware</strong>. No vendor lock-in. No per-site fees. Just your code on your server.
		</p>

		<div class="reveal fade-up delay-300 mt-10 flex flex-col sm:flex-row items-center justify-center gap-4">
			<button 
				onclick={() => openAuth('register')} 
				class="cta-btn w-full sm:w-auto inline-flex items-center justify-center gap-2 rounded-xl bg-gray-900 px-8 py-3.5 text-base font-bold text-white hover:bg-gray-800 transition-all shadow-md cursor-pointer"
			>
				Start Deploying Free
				<ChevronRight class="h-4 w-4" />
			</button>
			<a href="#how-it-works" onclick={smoothTo('how-it-works')} class="w-full sm:w-auto inline-flex items-center justify-center gap-2 rounded-xl border border-gray-200 bg-white/70 backdrop-blur px-8 py-3.5 text-base font-semibold text-gray-700 hover:bg-white transition-colors shadow-sm">
				See How It Works
			</a>
		</div>

		<!-- Terminal card -->
		<div class="reveal fade-up delay-400 mt-16 mx-auto max-w-3xl rounded-2xl border border-gray-200/80 bg-white/60 backdrop-blur-xl shadow-xl overflow-hidden">
			<div class="flex items-center gap-2 border-b border-gray-100 px-4 py-3 bg-gray-50/80">
				<div class="h-3 w-3 rounded-full bg-red-400/80"></div>
				<div class="h-3 w-3 rounded-full bg-yellow-400/80"></div>
				<div class="h-3 w-3 rounded-full bg-green-400/80"></div>
				<span class="font-mono text-[10px] text-gray-400 uppercase tracking-widest ml-3">Terminal Output</span>
			</div>
			<div class="p-6 text-left space-y-2 font-mono text-sm leading-relaxed bg-gray-950 text-gray-300">
				<p class="text-gray-500 typing-line" style="animation-delay:0.6s">$ git push origin main</p>
				<p class="text-blue-400 typing-line"  style="animation-delay:0.9s">→ Pulling latest changes from GitHub...</p>
				<p class="typing-line"                style="animation-delay:1.2s">→ Injecting .env variables...</p>
				<p class="text-yellow-400 typing-line" style="animation-delay:1.5s">→ Running build: bun run build</p>
				<p class="typing-line"                style="animation-delay:1.8s">→ Nginx configured and reloaded</p>
				<p class="text-green-400 typing-line" style="animation-delay:2.1s">→ SSL certificate deployed ✓</p>
				<p class="text-green-400 font-bold typing-line" style="animation-delay:2.4s">✅ Deployed successfully in 34s</p>
			</div>
		</div>
	</div>
</section>

<!-- ─── STATS BAR ──────────────────────────────────────────────── -->
<section class="border-y border-gray-200/60 py-10">
	<div class="mx-auto max-w-5xl px-6">
		<div class="grid grid-cols-1 sm:grid-cols-3 divide-y sm:divide-y-0 sm:divide-x divide-gray-200">
			{#each [['50+', 'Sites Deployed'], ['99.9%', 'Uptime'], ['< 60s', 'Avg Deploy Time']] as [val, label]}
				<div class="reveal fade-up text-center py-6 sm:py-0 sm:px-8">
					<p class="text-3xl font-black text-gray-900 tracking-tight">{val}</p>
					<p class="mt-1 text-sm font-medium text-gray-500">{label}</p>
				</div>
			{/each}
		</div>
	</div>
</section>

<!-- ─── FEATURES ──────────────────────────────────────────────── -->
<section id="features" class="py-28">
	<div class="mx-auto max-w-7xl px-6 lg:px-8">
		<div class="text-center mb-16 reveal fade-up">
			<p class="text-xs font-bold uppercase tracking-widest text-gray-400 mb-3">Features</p>
			<h2 class="text-3xl sm:text-4xl font-black text-gray-900 tracking-tight">
				Everything built-in.<br /><span class="text-gray-400">Nothing held back.</span>
			</h2>
			<p class="mt-4 text-gray-500 max-w-xl mx-auto">Vercel-grade deployments on your own hardware. Built for agencies with multiple client sites.</p>
		</div>

		<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5">
			{#each [
				{ Icon: Terminal, title: 'Live PM2 Logs', desc: "Stream live logs from your PM2 processes. Color-coded build output means you never have to SSH in to see what's happening." },
				{ Icon: Shield, title: 'Certbot Automation', desc: "Production-grade encryption via Let's Encrypt. Deploys SSL certs and handles HTTPS auto-renewal on your behalf." },
				{ Icon: Activity, title: 'Bare-Metal Monitoring', desc: 'Real-time CPU & RAM metrics straight from your Linux system. Know exactly how your server is breathing.' },
				{ Icon: Globe, title: 'Nginx Reverse Proxy', desc: 'SyncShip manages your Nginx configs. Point a domain, and we handle the routing, headers, and performance tweaks.' },
				{ Icon: GitBranch, title: 'One-Click Pipeline', desc: 'Push to GitHub, hit Deploy. We pull the code, install dependencies, build the assets, and restart the process.' },
				{ Icon: Rocket, title: 'Total BYOS Ownership', desc: "Bring Your Own Server. We don't host your data — you do. Zero lock-in, zero overage fees, 100% control." }
			] as { Icon, title, desc }, i}
				<div class="reveal fade-up feature-card group rounded-2xl border border-gray-200/80 bg-white/70 backdrop-blur-sm p-6 hover:shadow-lg transition-all duration-300 hover:-translate-y-1"
					style="animation-delay: {i * 80}ms">
					<div class="mb-4 inline-flex items-center justify-center rounded-xl bg-gray-100 p-3 group-hover:bg-gray-900 transition-colors duration-300">
						<Icon class="h-5 w-5 text-gray-700 group-hover:text-white transition-colors duration-300" />
					</div>
					<h3 class="text-base font-bold text-gray-900 mb-1.5">{title}</h3>
					<p class="text-sm text-gray-500 leading-relaxed">{desc}</p>
				</div>
			{/each}
		</div>
	</div>
</section>

<!-- ─── COMPARISON ───────────────────────────────────────────── -->
<section class="py-28 bg-gray-50/50">
	<div class="mx-auto max-w-5xl px-6">
		<div class="text-center mb-16 reveal fade-up">
			<h2 class="text-3xl font-black text-gray-900 tracking-tight">The Freedom Comparison</h2>
			<p class="mt-4 text-gray-500">Why thousands are coming back to their own servers.</p>
		</div>

		<div class="reveal fade-up overflow-x-auto rounded-2xl border border-gray-200 bg-white shadow-sm">
			<table class="w-full text-left border-collapse min-w-[600px] sm:min-w-0">
				<thead>
					<tr class="bg-gray-50/80">
						<th class="px-6 py-4 text-xs font-bold uppercase tracking-widest text-gray-400 border-b border-gray-100">feature</th>
						<th class="px-6 py-4 text-xs font-bold uppercase tracking-widest text-gray-400 border-b border-gray-100">Standard Cloud</th>
						<th class="px-6 py-4 text-xs font-bold uppercase tracking-widest text-indigo-600 border-b border-gray-100">SyncShip (BYOS)</th>
					</tr>
				</thead>
				<tbody class="divide-y divide-gray-100">
					<tr>
						<td class="px-6 py-4 text-sm font-semibold text-gray-900">Cost Structure</td>
						<td class="px-6 py-4 text-sm text-gray-500">Per-user / Per-site / Overage fees</td>
						<td class="px-6 py-4 text-sm text-gray-900 font-medium">Flat server fee (~$5/mo)</td>
					</tr>
					<tr>
						<td class="px-6 py-4 text-sm font-semibold text-gray-900">Data Ownership</td>
						<td class="px-6 py-4 text-sm text-gray-500">Hosted in their ecosystem</td>
						<td class="px-6 py-4 text-sm text-gray-900 font-medium">100% Your Infrastructure</td>
					</tr>
					<tr>
						<td class="px-6 py-4 text-sm font-semibold text-gray-900">Backend Support</td>
						<td class="px-6 py-4 text-sm text-gray-500">Serverless cold-starts & limits</td>
						<td class="px-6 py-4 text-sm text-gray-900 font-medium">Persistent PM2 processes</td>
					</tr>
					<tr>
						<td class="px-6 py-4 text-sm font-semibold text-gray-900">Environment</td>
						<td class="px-6 py-4 text-sm text-gray-500">Proprietary "Edge" runtime</td>
						<td class="px-6 py-4 text-sm text-gray-900 font-medium">Full Linux control</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</section>

<!-- ─── HOW IT WORKS ──────────────────────────────────────────── -->
<section id="how-it-works" class="py-28 lg:py-32 bg-white overflow-hidden border-y border-gray-100/70">
	<div class="mx-auto max-w-5xl px-6">
		<div class="text-center max-w-2xl mx-auto mb-20 reveal fade-up">
			<p class="text-xs font-bold uppercase tracking-widest text-indigo-600 mb-3">How It Works</p>
			<h2 class="text-3xl sm:text-4xl lg:text-5xl font-black text-gray-900 tracking-tight mb-4">From zero to deployed in 4 steps.</h2>
			<p class="text-gray-500 text-lg">You bring the server, we bring the magic. Here is exactly how you deploy your first project with SyncShip.</p>
		</div>

		<div class="relative before:absolute before:inset-0 before:ml-5 before:-translate-x-px md:before:mx-auto md:before:translate-x-0 before:h-full before:w-0.5 before:bg-gradient-to-b before:from-transparent before:via-gray-200 before:to-transparent">
			
			<!-- Step 1 -->
			<div class="relative flex items-center justify-between md:justify-normal md:odd:flex-row-reverse group mb-12 last:mb-0 reveal fade-up">
				<div class="flex items-center justify-center w-10 h-10 rounded-full border-4 border-white bg-indigo-50 shadow shrink-0 md:order-1 md:group-odd:-translate-x-1/2 md:group-even:translate-x-1/2 z-10">
					<Server class="text-indigo-600 w-5 h-5"/>
				</div>
				<div class="w-[calc(100%-4rem)] md:w-[calc(50%-3rem)] bg-white p-6 sm:p-8 rounded-2xl border border-gray-100 shadow-sm transition-all hover:shadow-md">
					<div class="flex items-center gap-3 mb-4">
						<span class="text-xs sm:text-sm font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-full whitespace-nowrap">Step 1</span>
						<h3 class="font-bold text-gray-900 text-lg sm:text-xl">Get a VPS</h3>
					</div>
					<p class="text-sm sm:text-base text-gray-500 leading-relaxed mb-6">You'll need a clean Ubuntu 20.04+ server to host your apps. A simple $4/month droplet from providers like DigitalOcean, Hetzner, or AWS works perfectly for most starter projects.</p>
					<div class="rounded-xl border border-gray-100 bg-gray-50 p-4 flex flex-wrap items-center gap-4">
						<img src="https://upload.wikimedia.org/wikipedia/commons/f/ff/DigitalOcean_logo.svg" alt="DigitalOcean" class="h-5 sm:h-6 opacity-60 grayscale hover:grayscale-0 transition-all"/>
						<span class="w-px h-6 bg-gray-200 hidden sm:block"></span>
						<img src="https://upload.wikimedia.org/wikipedia/commons/9/93/Amazon_Web_Services_Logo.svg" alt="AWS" class="h-5 sm:h-6 opacity-60 grayscale hover:grayscale-0 transition-all"/>
					</div>
				</div>
			</div>

			<!-- Step 2 -->
			<div class="relative flex items-center justify-between md:justify-normal md:odd:flex-row-reverse group mb-12 last:mb-0 reveal fade-up">
				<div class="flex items-center justify-center w-10 h-10 rounded-full border-4 border-white bg-indigo-50 shadow shrink-0 md:order-1 md:group-odd:-translate-x-1/2 md:group-even:translate-x-1/2 z-10">
					<Key class="text-indigo-600 w-5 h-5"/>
				</div>
				<div class="w-[calc(100%-4rem)] md:w-[calc(50%-3rem)] bg-white p-6 sm:p-8 rounded-2xl border border-gray-100 shadow-sm transition-all hover:shadow-md">
					<div class="flex items-center gap-3 mb-4">
						<span class="text-xs sm:text-sm font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-full whitespace-nowrap">Step 2</span>
						<h3 class="font-bold text-gray-900 text-lg sm:text-xl">Generate Server Key</h3>
					</div>
					<p class="text-sm sm:text-base text-gray-500 leading-relaxed mb-6">Once you create a free SyncShip account, head over to your settings dashboard. Click "Generate API Token" to create a secure key that will bind your server to our platform.</p>
					<div class="rounded-xl border border-gray-100 bg-gray-50 p-3 sm:p-4">
						<div class="flex flex-col sm:flex-row items-start sm:items-center justify-between bg-white border border-gray-200 rounded-lg p-3 shadow-sm gap-3 sm:gap-0">
							<div class="flex items-center gap-2">
								<Key class="w-4 h-4 text-gray-400"/>
								<span class="text-xs sm:text-sm font-mono text-gray-600">sync_sec_9x8f...</span>
							</div>
							<button class="w-full sm:w-auto text-[10px] sm:text-xs font-bold text-indigo-600 bg-indigo-50 px-3 py-1.5 rounded-md hover:bg-indigo-100 transition-colors">Generate Token</button>
						</div>
					</div>
				</div>
			</div>

			<!-- Step 3 -->
			<div class="relative flex items-center justify-between md:justify-normal md:odd:flex-row-reverse group mb-12 last:mb-0 reveal fade-up">
				<div class="flex items-center justify-center w-10 h-10 rounded-full border-4 border-white bg-indigo-50 shadow shrink-0 md:order-1 md:group-odd:-translate-x-1/2 md:group-even:translate-x-1/2 z-10">
					<Terminal class="text-indigo-600 w-5 h-5"/>
				</div>
				<div class="w-[calc(100%-4rem)] md:w-[calc(50%-3rem)] bg-white p-6 sm:p-8 rounded-2xl border border-gray-100 shadow-sm transition-all hover:shadow-md">
					<div class="flex items-center gap-3 mb-4">
						<span class="text-xs sm:text-sm font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-full whitespace-nowrap">Step 3</span>
						<h3 class="font-bold text-gray-900 text-lg sm:text-xl">Install the Daemon</h3>
					</div>
					<p class="text-sm sm:text-base text-gray-500 leading-relaxed mb-6">SSH into your newly created VPS and paste our one-line installer. It authenticates with your key, installs Nginx and Node, and securely connects back to SyncShip instantly.</p>
					<div class="bg-gray-950 rounded-xl p-3 sm:p-4 overflow-hidden shadow-inner hidden xs:block">
						<div class="flex items-center gap-1.5 mb-3">
							<div class="h-2 w-2 sm:h-2.5 sm:w-2.5 rounded-full bg-red-500/80"></div>
							<div class="h-2 w-2 sm:h-2.5 sm:w-2.5 rounded-full bg-yellow-500/80"></div>
							<div class="h-2 w-2 sm:h-2.5 sm:w-2.5 rounded-full bg-green-500/80"></div>
							<span class="text-[8px] sm:text-[10px] text-gray-500 font-mono ml-2 uppercase tracking-wider">root@ubuntu:~</span>
						</div>
						<code class="text-[10px] sm:text-xs text-blue-400 font-mono flex opacity-90 leading-relaxed break-all">
							<span class="text-gray-500 mr-2 select-none">$</span>
							curl -s https://syncship.ink/install.sh | bash
						</code>
						<code class="text-[10px] sm:text-xs text-green-400 font-mono flex flex-wrap opacity-90 leading-relaxed mt-2 sm:mt-3 break-words">
							→ SyncShip Daemon securely connected!
						</code>
					</div>
				</div>
			</div>

			<!-- Step 4 -->
			<div class="relative flex items-center justify-between md:justify-normal md:odd:flex-row-reverse group mb-12 last:mb-0 reveal fade-up">
				<div class="flex items-center justify-center w-10 h-10 rounded-full border-4 border-white bg-indigo-50 shadow shrink-0 md:order-1 md:group-odd:-translate-x-1/2 md:group-even:translate-x-1/2 z-10">
					<Rocket class="text-indigo-600 w-5 h-5"/>
				</div>
				<div class="w-[calc(100%-4rem)] md:w-[calc(50%-3rem)] bg-white p-6 sm:p-8 rounded-2xl border border-gray-100 shadow-sm transition-all hover:shadow-md">
					<div class="flex items-center gap-3 mb-4">
						<span class="text-xs sm:text-sm font-bold text-indigo-600 bg-indigo-50 px-2.5 py-1 rounded-full whitespace-nowrap">Step 4</span>
						<h3 class="font-bold text-gray-900 text-lg sm:text-xl">Deploy!</h3>
					</div>
					<p class="text-sm sm:text-base text-gray-500 leading-relaxed mb-6">Your dashboard will show your server as "Online". Now, just paste your GitHub repo URL and click deploy. We'll pull your code, build it, and assign SSL magic. You're live!</p>
					<div class="rounded-xl border border-gray-100 bg-gray-50 p-2 overflow-hidden">
						<div class="bg-white border border-gray-200 rounded-lg p-3 sm:p-4 shadow-sm">
							<div class="flex items-center justify-between mb-3 border-b border-gray-100 pb-3">
								<span class="text-xs sm:text-sm font-bold text-gray-900">New Project</span>
								<span class="flex items-center gap-1.5 text-[10px] sm:text-xs font-medium text-green-600 bg-green-50 px-2 py-0.5 rounded-full"><span class="w-1.5 h-1.5 rounded-full bg-green-500"></span> Online</span>
							</div>
							<button class="w-full bg-gray-900 hover:bg-gray-800 transition-colors text-white text-[11px] sm:text-sm font-bold py-2 sm:py-2.5 rounded-lg flex items-center justify-center gap-2">
								<Rocket class="w-3.5 h-3.5"/> Deploy to Production
							</button>
						</div>
					</div>
				</div>
			</div>

		</div>
	</div>
</section>

<!-- ─── PRICING ───────────────────────────────────────────────── -->
<section id="pricing" class="py-28">
	<div class="mx-auto max-w-4xl px-6">
		<div class="text-center mb-16 reveal fade-up">
			<p class="text-xs font-bold uppercase tracking-widest text-gray-400 mb-3">Pricing</p>
			<h2 class="text-3xl sm:text-4xl font-black text-gray-900 tracking-tight">
				Unlimited Scale.<br /><span class="text-gray-400">Fixed Tiny Cost.</span>
			</h2>
			<p class="mt-4 text-gray-500 max-w-md mx-auto">
				Pay for the tool once. Pay your server provider directly for the iron. No middleman markup. No bandwidth tax.
			</p>
		</div>

		<div class="grid grid-cols-1 md:grid-cols-2 gap-6 max-w-3xl mx-auto">

			<!-- Founding Member Lifetime -->
			<div class="reveal fade-up delay-100 rounded-2xl border border-gray-900 bg-gray-900 p-8 flex flex-col relative overflow-hidden shadow-2xl">
				<!-- Founding badge -->
				<div class="absolute top-4 right-4 rounded-full bg-white/10 px-2.5 py-1 text-[10px] font-bold text-white uppercase tracking-wider">
					Founding Member
				</div>
				<p class="text-xs font-bold uppercase tracking-widest text-indigo-400">Temporary Lifetime Access Fee</p>
				<div class="mt-5 flex items-end gap-2">
					<span class="text-5xl font-black text-white">$2</span>
					<span class="text-gray-400 mb-1.5 text-sm line-through">$59</span>
				</div>
				<p class="mt-2 text-sm text-gray-400">Limited time promotion. Full lifetime access.</p>

				<!-- Urgency bar -->
				<div class="mt-5 rounded-xl bg-white/5 border border-white/10 px-4 py-3">
					<div class="flex items-center justify-between mb-1.5">
						<span class="text-xs text-gray-400 font-medium">Founding seats</span>
						<span class="text-xs font-bold text-white">38 / 50 remaining</span>
					</div>
					<div class="w-full bg-white/10 rounded-full h-1.5 overflow-hidden">
						<div class="bg-white h-1.5 rounded-full" style="width: 24%"></div>
					</div>
				</div>

				<ul class="mt-6 space-y-3 flex-1">
					{#each [
						'Unlimited Droplets',
						'Unlimited Sites',
						'Live Deployment Logs',
						'Auto SSL Certificates',
						'Real-time Server Stats',
						'All future features included'
					] as f}
						<li class="flex items-center gap-2 text-sm text-gray-300">
							<Check class="h-4 w-4 text-white/60 shrink-0" />{f}
						</li>
					{/each}
				</ul>

				<a
					href="https://buy.polar.sh/polar_cl_wI21HXqPa8S1S3W6Y9lBK0cuwU0gNfPdjUb9l4HgmLO{$currentUser?.email ? `?customer_email=${$currentUser.email}` : ''}"
					target="_blank"
					rel="noopener noreferrer"
					class="mt-8 block rounded-xl bg-white py-3 text-center text-sm font-bold text-gray-900 hover:bg-gray-100 transition-colors shadow"
				>
					Get Lifetime Access →
				</a>
				<p class="mt-3 text-center text-[11px] text-gray-500">Secure checkout via Polar · No subscription</p>
			</div>

			<!-- Pro Monthly (coming soon after founding round) -->
			<div class="reveal fade-up delay-200 rounded-2xl border border-gray-200/70 bg-white/70 backdrop-blur p-8 flex flex-col shadow-sm">
				<p class="text-xs font-bold uppercase tracking-widest text-gray-400">Pro Monthly</p>
				<div class="mt-5 flex items-end gap-1">
					<span class="text-5xl font-black text-gray-900">$19</span>
					<span class="text-gray-400 mb-1.5 text-sm">/month</span>
				</div>
				<p class="mt-2 text-sm text-gray-500">Available after founding member seats are filled.</p>

				<!-- Coming soon overlay feel -->
				<div class="mt-5 rounded-xl border border-gray-200/60 bg-gray-50/80 px-4 py-3 text-center">
					<span class="text-xs font-semibold text-gray-500 uppercase tracking-wide">Opens when founding round closes</span>
				</div>

				<ul class="mt-6 space-y-3 flex-1 opacity-60">
					{#each [
						'Unlimited Droplets',
						'Unlimited Sites',
						'Live Deployment Logs',
						'Auto SSL Certificates',
						'Real-time Server Stats',
						'Cancel anytime'
					] as f}
						<li class="flex items-center gap-2 text-sm text-gray-600">
							<Check class="h-4 w-4 text-gray-300 shrink-0" />{f}
						</li>
					{/each}
				</ul>

				<button disabled class="mt-8 block w-full rounded-xl border border-gray-200 bg-gray-100 py-3 text-center text-sm font-semibold text-gray-400 cursor-not-allowed">
					Not available yet
				</button>
				<p class="mt-3 text-center text-[11px] text-gray-400">Get the lifetime deal while it lasts ↑</p>
			</div>
		</div>
	</div>
</section>

<!-- ─── CTA ───────────────────────────────────────────────────── -->
<section class="border-t border-gray-100/70 py-24">
	<div class="mx-auto max-w-3xl px-6 text-center reveal fade-up">
		<div class="inline-flex items-center justify-center rounded-2xl bg-gray-900 p-4 mb-6 shadow-md">
			<Droplets class="h-8 w-8 text-white" />
		</div>
		<h2 class="text-3xl sm:text-4xl font-black text-gray-900 tracking-tight">Ready to ship faster?</h2>
		<p class="mt-4 text-gray-500 max-w-md mx-auto">Join agencies already using SyncShip to deploy client sites on their own terms.</p>
		<button 
			onclick={() => openAuth('register')} 
			class="cta-btn mt-8 inline-flex items-center gap-2 rounded-xl bg-gray-900 px-10 py-3.5 text-base font-bold text-white hover:bg-gray-800 transition-all shadow-md cursor-pointer"
		>
			Get Started Free <ChevronRight class="h-4 w-4" />
		</button>
	</div>
</section>

<!-- ─── FOOTER ────────────────────────────────────────────────── -->
<footer class="border-t border-gray-200/60 py-10">
	<div class="mx-auto max-w-7xl px-6">
		<div class="flex flex-col md:flex-row items-center justify-between gap-5">
			<a href="/" class="flex items-center gap-x-2.5">
				<div class="rounded-lg bg-gray-900 p-1.5">
					<Droplets class="h-4 w-4 text-white" />
				</div>
				<span class="text-sm font-bold tracking-tight text-gray-900">SyncShip</span>
				<span class="text-gray-400 text-xs ml-1">— Ship your clients' sites, not your sanity.</span>
			</a>
			<div class="flex items-center gap-6">
				<a href="#features"     onclick={smoothTo('features')}     class="text-xs text-gray-400 hover:text-gray-700 transition-colors">Features</a>
				<a href="#how-it-works" onclick={smoothTo('how-it-works')} class="text-xs text-gray-400 hover:text-gray-700 transition-colors">How it works</a>
				<a href="#pricing"      onclick={smoothTo('pricing')}      class="text-xs text-gray-400 hover:text-gray-700 transition-colors">Pricing</a>
				<button 
					onclick={() => openAuth('login')} 
					class="text-xs text-gray-400 hover:text-gray-700 transition-colors cursor-pointer"
				>
					Sign In
				</button>
			</div>
		</div>
		<div class="mt-8 pt-6 border-t border-gray-200 flex flex-col sm:flex-row items-center justify-between gap-4">
			<p class="text-xs text-gray-400">© 2026 SyncShip. Built with ♥ by <span class="font-bold text-gray-600">Kelly Owoju</span> for agencies.</p>
			<div class="flex items-center gap-8">
				<a href="/privacy" class="text-sm font-medium text-gray-500 hover:text-gray-900 transition-colors underline decoration-gray-200 underline-offset-4 hover:decoration-gray-400">Privacy Policy</a>
				<a href="/terms" class="text-sm font-medium text-gray-500 hover:text-gray-900 transition-colors underline decoration-gray-200 underline-offset-4 hover:decoration-gray-400">Terms & Conditions</a>
			</div>
		</div>
	</div>
</footer>

<AuthModal bind:show={showAuthModal} mode={authMode} />

<style>
	/* ── Smooth scroll ───────────── */
	:global(html) { scroll-behavior: smooth; }

	/* ── Scroll reveal ───────────── */
	:global(.reveal) {
		opacity: 0;
		transform: translateY(28px);
		transition: opacity 0.65s cubic-bezier(0.22,1,0.36,1),
		            transform 0.65s cubic-bezier(0.22,1,0.36,1);
	}
	:global(.reveal.revealed) {
		opacity: 1;
		transform: translateY(0);
	}
	:global(.fade-up) { transform: translateY(28px); }
	:global(.delay-100) { transition-delay: 0.10s; }
	:global(.delay-200) { transition-delay: 0.20s; }
	:global(.delay-300) { transition-delay: 0.30s; }
	:global(.delay-400) { transition-delay: 0.40s; }

	/* ── Terminal line fade-in ───── */
	:global(.typing-line) {
		opacity: 0;
		animation: line-appear 0.45s ease forwards;
	}
	@keyframes line-appear {
		from { opacity: 0; transform: translateX(-6px); }
		to   { opacity: 1; transform: translateX(0); }
	}

	/* ── CTA button pulse hover ──── */
	:global(.cta-btn) {
		position: relative;
		overflow: hidden;
	}
	:global(.cta-btn::after) {
		content: '';
		position: absolute;
		inset: 0;
		background: rgba(255,255,255,0.08);
		opacity: 0;
		transition: opacity 0.2s;
	}
	:global(.cta-btn:hover::after) { opacity: 1; }
	:global(.cta-btn:active) { transform: scale(0.97); }

	/* ── Feature card shine effect ─ */
	:global(.feature-card) {
		position: relative;
		overflow: hidden;
	}
	:global(.feature-card::before) {
		content: '';
		position: absolute;
		top: -60%; left: -60%;
		width: 50%; height: 50%;
		background: radial-gradient(circle, rgba(255,255,255,0.35) 0%, transparent 70%);
		transform: rotate(-30deg);
		opacity: 0;
		transition: opacity 0.4s, transform 0.6s;
		pointer-events: none;
	}
	:global(.feature-card:hover::before) {
		opacity: 1;
		transform: rotate(-30deg) translate(60%, 60%);
	}

	/* ── Hero glass background ── */
	:global(.hero-section) {
		background: linear-gradient(160deg, #e6ecf0 0%, #eef2f5 45%, #e2e8ed 100%);
	}
	:global(.hero-noise) {
		background: url("data:image/svg+xml,%3Csvg viewBox='0 0 512 512' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.75' numOctaves='4' stitchTiles='stitch'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E") center / 512px;
		opacity: 0.035;
	}
	:global(.hero-shimmer) {
		background: linear-gradient(105deg, rgba(255,255,255,0.55) 0%, rgba(255,255,255,0) 40%, rgba(255,255,255,0.25) 60%, rgba(255,255,255,0) 100%);
	}
	:global(.hero-fog) {
		background: linear-gradient(to top, rgba(230,236,240,0.8), transparent);
	}
</style>
