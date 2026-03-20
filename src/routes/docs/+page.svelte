<script lang="ts">
	import { onMount } from 'svelte';
	import { 
		Droplets, Terminal, Shield, Rocket, Server, Key, 
		GitBranch, Globe, BookOpen, ChevronRight, Check, 
		Copy, AlertCircle, Cpu, HardDrive, Activity
	} from 'lucide-svelte';
	import { fade, fly } from 'svelte/transition';

	let activeSection = $state('introduction');
	let scrolled = $state(false);

	const sections = [
		{ id: 'introduction', title: 'Introduction', icon: BookOpen },
		{ id: 'installation', title: 'Installation', icon: Terminal },
		{ id: 'github', title: 'GitHub Integration', icon: GitBranch },
		{ id: 'deployment', title: 'Deployment Guide', icon: Rocket },
		{ id: 'domains', title: 'Domains & SSL', icon: Globe },
		{ id: 'advanced', title: 'Advanced Config', icon: Shield },
	];

	function scrollTo(id: string) {
		activeSection = id;
		const el = document.getElementById(id);
		if (el) {
			const yOffset = -100;
			const y = el.getBoundingClientRect().top + window.pageYOffset + yOffset;
			window.scrollTo({ top: y, behavior: 'smooth' });
		}
	}

	onMount(() => {
		const handleScroll = () => {
			scrolled = window.scrollY > 20;
			
			// Update active section based on scroll position
			for (const section of sections) {
				const el = document.getElementById(section.id);
				if (el) {
					const rect = el.getBoundingClientRect();
					if (rect.top <= 150 && rect.bottom >= 150) {
						activeSection = section.id;
						break;
					}
				}
			}
		};

		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	});

	function copyToClipboard(text: string) {
		navigator.clipboard.writeText(text);
		// toast or feedback here
	}
</script>

<svelte:head>
	<title>Documentation | SyncShip</title>
	<meta name="description" content="SyncShip developer documentation. Learn how to deploy your apps on your own server." />
</svelte:head>

<div class="min-h-screen bg-gray-50/30 text-gray-900 font-sans selection:bg-indigo-100 selection:text-indigo-900">
	
	<!-- ─── HEADER ────────────────────────────────────────────────── -->
	<header class="fixed top-0 inset-x-0 z-50 transition-all duration-300 border-b {scrolled ? 'bg-white/80 backdrop-blur-xl border-gray-200 shadow-sm' : 'bg-transparent border-transparent'}">
		<div class="mx-auto max-w-7xl px-6 h-16 flex items-center justify-between">
			<a href="/" class="flex items-center gap-x-2.5">
				<div class="rounded-lg bg-gray-900 p-1.5">
					<Droplets class="h-4 w-4 text-white" />
				</div>
				<span class="text-lg font-bold tracking-tight text-gray-900">SyncShip</span>
				<span class="text-xs font-bold bg-indigo-50 text-indigo-600 px-2 py-0.5 rounded-full ml-2">Docs</span>
			</a>
			
			<div class="flex items-center gap-6">
				<a href="/dashboard" class="rounded-lg bg-gray-900 px-4 py-2 text-sm font-bold text-white hover:bg-gray-800 transition-all shadow-sm">
					Dashboard
				</a>
			</div>
		</div>
	</header>

	<div class="mx-auto max-w-7xl px-6 pt-24 pb-20">
		<div class="flex flex-col lg:flex-row gap-12">
			
			<!-- ─── SIDEBAR ──────────────────────────────────────────────── -->
			<aside class="hidden lg:block w-64 shrink-0 sticky top-24 h-fit">
				<nav class="space-y-1">
					{#each sections as section}
						<button 
							onclick={() => scrollTo(section.id)}
							class="w-full flex items-center gap-3 px-4 py-3 rounded-xl transition-all duration-300 text-left
								{activeSection === section.id 
									? 'bg-white text-indigo-600 shadow-sm ring-1 ring-gray-900/5 font-bold' 
									: 'text-gray-500 hover:text-gray-900 hover:bg-gray-100/50'}"
						>
							<section.icon class="w-4 h-4" />
							<span class="text-sm">{section.title}</span>
							{#if activeSection === section.id}
								<ChevronRight class="w-3.5 h-3.5 ml-auto opacity-50" />
							{/if}
						</button>
					{/each}
				</nav>


			</aside>

			<!-- ─── MAIN CONTENT ────────────────────────────────────────── -->
			<main class="flex-1 max-w-3xl space-y-24">
				
				<!-- INTRODUCTION -->
				<section id="introduction" class="scroll-mt-32">
					<div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-indigo-50 text-indigo-600 text-[10px] font-bold uppercase tracking-widest mb-6">
						Getting Started
					</div>
					<h1 class="text-4xl sm:text-5xl font-black text-gray-900 tracking-tight leading-[1.1] mb-6">
						Own Your Infrastructure.
					</h1>
					<div class="prose prose-indigo max-w-none text-gray-500 leading-relaxed space-y-6">
						<p class="text-lg text-gray-600 font-medium">
							SyncShip is a self-hosted Platform-as-a-Service (PaaS) designed for developers and agencies who are tired of high bandwidth bills and proprietary "Edge" platform limits.
						</p>
						<p>
							By installing a lightweight daemon on your own Ubuntu VPS, you gain the "Vercel Experience" — git-push to deploy, automated Nginx routing, and Let's Encrypt SSL — without the markup. SyncShip manages the DevOps heavy lifting, so you can focus on building beautiful products.
						</p>
					</div>

					<!-- Feature Highlight Grid -->
					<div class="mt-12 grid grid-cols-1 sm:grid-cols-2 gap-4">
						<div class="p-5 rounded-2xl border border-gray-200/80 bg-white/50 backdrop-blur-sm">
							<div class="w-8 h-8 rounded-lg bg-green-50 text-green-600 flex items-center justify-center mb-4">
								<Check class="w-4 h-4"/>
							</div>
							<h4 class="font-bold text-gray-900 text-sm mb-1">Zero Lock-in</h4>
							<p class="text-xs text-gray-500 leading-relaxed">It's your server. If you stop using SyncShip, your apps keep running on standard Nginx/PM2.</p>
						</div>
						<div class="p-5 rounded-2xl border border-gray-200/80 bg-white/50 backdrop-blur-sm">
							<div class="w-8 h-8 rounded-lg bg-blue-50 text-blue-600 flex items-center justify-center mb-4">
								<Cpu class="w-4 h-4"/>
							</div>
							<h4 class="font-bold text-gray-900 text-sm mb-1">Raw Performance</h4>
							<p class="text-xs text-gray-500 leading-relaxed">No "cold starts". Your backend apps run as persistent processes with full access to host resources.</p>
						</div>
					</div>
				</section>

				<!-- INSTALLATION -->
				<section id="installation" class="scroll-mt-32">
					<h2 class="text-3xl font-black text-gray-900 tracking-tight mb-8 flex items-center gap-3">
						<Terminal class="w-8 h-8 text-indigo-600"/> Installation
					</h2>
					<div class="space-y-8">
						<div class="p-6 rounded-2xl bg-gray-950 text-gray-300 font-mono text-sm relative group">
							<div class="absolute right-4 top-4 opacity-0 group-hover:opacity-100 transition-opacity">
								<button 
									onclick={() => copyToClipboard('curl -s https://syncship.ink/install.sh | bash')}
									class="p-2 rounded-lg bg-white/10 hover:bg-white/20 text-white transition-colors"
								>
									<Copy class="w-4 h-4"/>
								</button>
							</div>
							<span class="text-gray-500"># 1. SSH into your Ubuntu VPS (20.04+) as root</span>
							<br/>
							<span class="text-gray-500"># 2. Run the one-line installer</span>
							<br/>
							<div class="mt-2 text-indigo-400">
								export SYNC_DAEMON_TOKEN="your_daemon_token"
							</div>
							<div class="mt-1">
								curl -s https://syncship.ink/install.sh | bash
							</div>
						</div>

						<div class="grid grid-cols-1 md:grid-cols-2 gap-8">
							<div class="space-y-4">
								<h4 class="font-bold text-gray-900">What happens during install?</h4>
								<ul class="space-y-3">
									{#each [
										'Installs generic dependencies (Node.js, PM2, Git)',
										'Configures Nginx and initializes the web root',
										'Sets up the SyncShip Daemon as a systemd service',
										'Establishes a secure Firestore connection for real-time control'
									] as step}
										<li class="flex items-start gap-2.5 text-sm text-gray-500 leading-relaxed">
											<div class="mt-1 shrink-0 w-1.5 h-1.5 rounded-full bg-indigo-500"></div>
											{step}
										</li>
									{/each}
								</ul>
							</div>
							<div>
								<div class="p-5 rounded-2xl bg-amber-50 border border-amber-100">
									<h4 class="font-bold text-amber-900 text-sm flex items-center gap-2 mb-2">
										<AlertCircle class="w-4 h-4"/> Prerequisites
									</h4>
									<p class="text-xs text-amber-800 leading-relaxed">
										Ensure your VPS firewall allows incoming traffic on **Port 80 (HTTP)** and **Port 443 (HTTPS)**. SyncShip needs these for Nginx routing and Certbot SSL generation.
									</p>
								</div>
							</div>
						</div>
					</div>
				</section>

				<!-- GITHUB INTEGRATION -->
				<section id="github" class="scroll-mt-32">
					<h2 class="text-3xl font-black text-gray-900 tracking-tight mb-8 flex items-center gap-3">
						<GitBranch class="w-8 h-8 text-indigo-600"/> GitHub Integration
					</h2>
					<div class="prose prose-indigo max-w-none text-gray-500 text-sm leading-relaxed space-y-6">
						<p>
							SyncShip connects directly to your GitHub account to pull your source code and listen for updates. We use secure OAuth authentication and scoped permissions to ensure your repos are safe.
						</p>
						<p>
							Once connected, you can browse and select any repository — including private ones. SyncShip auto-injects an access token during the deployment phase, so you never have to manage SSH keys on your VPS manually.
						</p>
						<div class="flex items-center gap-4 py-4">
							<div class="flex-1 p-4 rounded-xl border border-gray-200 bg-white flex items-center gap-3">
								<div class="w-10 h-10 rounded-lg bg-gray-50 flex items-center justify-center">
									<Shield class="w-5 h-5 text-gray-400"/>
								</div>
								<div>
									<h5 class="font-bold text-gray-900 text-sm">Fine-grained Scopes</h5>
									<p class="text-[10px] text-gray-500">We only request repo read access.</p>
								</div>
							</div>
							<div class="flex-1 p-4 rounded-xl border border-gray-200 bg-white flex items-center gap-3">
								<div class="w-10 h-10 rounded-lg bg-gray-50 flex items-center justify-center">
									<Rocket class="w-5 h-5 text-gray-400"/>
								</div>
								<div>
									<h5 class="font-bold text-gray-900 text-sm">Auto-Webhooks</h5>
									<p class="text-[10px] text-gray-500">GitHub notifies us on every push.</p>
								</div>
							</div>
						</div>
					</div>
				</section>

				<!-- DEPLOYMENT GUIDE -->
				<section id="deployment" class="scroll-mt-32">
					<h2 class="text-3xl font-black text-gray-900 tracking-tight mb-8 flex items-center gap-3">
						<Rocket class="w-8 h-8 text-indigo-600"/> Deployment Guide
					</h2>
					<div class="space-y-12">
						
						<!-- Static Sites -->
						<div>
							<h3 class="text-xl font-bold text-gray-900 mb-4 flex items-center gap-2">
								<Globe class="w-5 h-5 text-indigo-600"/> Static Sites
							</h3>
							<p class="text-sm text-gray-500 leading-relaxed mb-6">
								Perfect for SvelteKit, Vite, React, or static generators like Hugo/Astro. SyncShip builds your app and routes Nginx to the output directory.
							</p>
							<div class="grid grid-cols-1 md:grid-cols-3 gap-4">
								<div class="p-4 rounded-xl border border-gray-100 bg-white">
									<label class="block text-[10px] font-bold text-gray-400 uppercase tracking-widest mb-1">Build Command</label>
									<code class="text-xs text-indigo-600 font-bold">npm run build</code>
								</div>
								<div class="p-4 rounded-xl border border-gray-100 bg-white">
									<label class="block text-[10px] font-bold text-gray-400 uppercase tracking-widest mb-1">Output Directory</label>
									<code class="text-xs text-indigo-600 font-bold">dist</code>
								</div>
								<div class="p-4 rounded-xl border border-gray-100 bg-white">
									<label class="block text-[10px] font-bold text-gray-400 uppercase tracking-widest mb-1">Engine</label>
									<code class="text-xs text-indigo-600 font-bold">standard</code>
								</div>
							</div>
						</div>

						<!-- Backend Apps -->
						<div class="pt-8 border-t border-gray-100">
							<h3 class="text-xl font-bold text-gray-900 mb-4 flex items-center gap-2">
								<Server class="w-5 h-5 text-indigo-600"/> Node.js Backend
							</h3>
							<p class="text-sm text-gray-500 leading-relaxed mb-6">
								For persistent API servers or SSR apps. SyncShip manages the process via **PM2** and automatically handles the reverse proxying to an available host port.
							</p>
							<div class="bg-indigo-50/50 rounded-2xl p-6 border border-indigo-100/50">
								<h4 class="text-sm font-bold text-indigo-900 mb-3">Automatic Port Allocation</h4>
								<p class="text-xs text-indigo-700 leading-relaxed">
									SyncShip scans your system for used ports (including Docker and other PM2 processes) and allocates a unique port starting from **3001**. This port is then injected via the `PORT` environment variable.
								</p>
							</div>
						</div>

						<!-- Docker -->
						<div class="pt-8 border-t border-gray-100">
							<h3 class="text-xl font-bold text-gray-900 mb-4 flex items-center gap-2">
								<HardDrive class="w-5 h-5 text-indigo-600"/> Docker & Compose
							</h3>
							<p class="text-sm text-gray-500 leading-relaxed mb-6">
								The most flexible way to deploy. Just include a `docker-compose.yml` in your repo. SyncShip will run `docker-compose up -d --build` and route your domain to the correct host port.
							</p>
							<div class="p-6 rounded-2xl bg-gray-950 text-gray-300 font-mono text-sm">
								<span class="text-gray-500"># Use the $PORT variable in your compose file</span>
								<br/>
								<span>ports:</span>
								<br/>
								<span>  - "<span class="text-yellow-400">{"${PORT}"}</span>:3000"</span>
							</div>
						</div>

					</div>
				</section>

				<!-- DOMAINS & SSL -->
				<section id="domains" class="scroll-mt-32">
					<h2 class="text-3xl font-black text-gray-900 tracking-tight mb-8 flex items-center gap-3">
						<Globe class="w-8 h-8 text-indigo-600"/> Domains & SSL
					</h2>
					<div class="space-y-8">
						<div class="prose prose-indigo max-w-none text-gray-500 text-sm leading-relaxed">
							<p>
								SyncShip treats second-level domains as first-class citizens. When you add a domain, our daemon automatically generates an Nginx configuration file in `/etc/nginx/sites-available/` and links it to `sites-enabled`.
							</p>
						</div>

						<div class="grid grid-cols-1 sm:grid-cols-2 gap-6">
							<div class="p-6 rounded-2xl border border-gray-200 bg-white">
								<h4 class="font-bold text-gray-900 text-sm mb-3">1. Point Your DNS</h4>
								<p class="text-xs text-gray-500 leading-relaxed mb-4">Create an **A Record** pointing to your Droplet IP.</p>
								<div class="flex items-center gap-2 text-xs font-mono bg-gray-50 px-3 py-2 rounded-lg border border-gray-100">
									<span class="text-gray-400">A</span> 
									<span>app.test.com</span>
									<span class="text-indigo-600 ml-auto">123.45.67.89</span>
								</div>
							</div>
							<div class="p-6 rounded-2xl border border-gray-200 bg-white">
								<h4 class="font-bold text-gray-900 text-sm mb-3">2. Auto-SSL (Certbot)</h4>
								<p class="text-xs text-gray-500 leading-relaxed mb-4">SyncShip triggers Certbot automatically once DNS propagates.</p>
								<div class="flex items-center gap-2 py-1 px-3 bg-green-50 text-green-700 rounded-full w-fit">
									<Shield class="w-3 h-3"/>
									<span class="text-[10px] font-bold">HTTPS Secured via Let's Encrypt</span>
								</div>
							</div>
						</div>
					</div>
				</section>

				<!-- ADVANCED CONFIG -->
				<section id="advanced" class="scroll-mt-32">
					<h2 class="text-3xl font-black text-gray-900 tracking-tight mb-8 flex items-center gap-3">
						<Shield class="w-8 h-8 text-indigo-600"/> Advanced Configuration
					</h2>
					<div class="space-y-12">
						
						<!-- ENV VARS -->
						<div>
							<h3 class="text-xl font-bold text-gray-900 mb-4 flex items-center gap-2">
								<Key class="w-5 h-5 text-indigo-600"/> Secrets & Env Vars
							</h3>
							<p class="text-sm text-gray-500 leading-relaxed">
								Manage sensitive data without committing it to Git. Add your keys in the **Site Settings** dashboard. SyncShip will create a local `.env` file in your project root before every build.
							</p>
						</div>

						<!-- WEBHOOKS -->
						<div class="pt-8 border-t border-gray-100">
							<h3 class="text-xl font-bold text-gray-900 mb-4 flex items-center gap-2">
								<GitBranch class="w-5 h-5 text-indigo-600"/> Automated Webhooks
							</h3>
							<p class="text-sm text-gray-500 leading-relaxed">
								SyncShip is built for CI/CD. When you connect a GitHub repo, we automatically register a webhook. Every time you `git push`, SyncShip's daemon pulls the latest code and triggers a re-deploy.
							</p>
						</div>

						<!-- MONITORING -->
						<div class="pt-8 border-t border-gray-100">
							<h3 class="text-xl font-bold text-gray-900 mb-4 flex items-center gap-2">
								<Activity class="w-5 h-5 text-indigo-600"/> Real-time Monitoring
							</h3>
							<div class="grid grid-cols-2 gap-4">
								<div class="p-4 rounded-xl bg-gray-50 border border-gray-100 flex items-center gap-3">
									<Cpu class="w-5 h-5 text-gray-400"/>
									<div>
										<h5 class="font-bold text-gray-900 text-xs">CPU Usage</h5>
										<p class="text-[10px] text-gray-500">Live per-core metrics.</p>
									</div>
								</div>
								<div class="p-4 rounded-xl bg-gray-50 border border-gray-100 flex items-center gap-3">
									<HardDrive class="w-5 h-5 text-gray-400"/>
									<div>
										<h5 class="font-bold text-gray-900 text-xs">RAM Stats</h5>
										<p class="text-[10px] text-gray-500">Memory footprint tracking.</p>
									</div>
								</div>
							</div>
						</div>

					</div>
				</section>

				<!-- FOOTER NAVIGATION -->
				<div class="pt-20 border-t border-gray-200 flex flex-col sm:flex-row items-center justify-between gap-6 opacity-60 grayscale hover:grayscale-0 transition-all duration-500">
					<div class="flex items-center gap-3">
						<Droplets class="h-6 w-6 text-gray-900" />
						<div class="text-xs font-bold text-gray-900 uppercase tracking-widest">End of the line.</div>
					</div>
					<a href="/dashboard" class="flex items-center gap-2 text-sm font-bold text-indigo-600 hover:text-indigo-800 transition-colors">
						Back to Dashboard <ChevronRight class="w-4 h-4"/>
					</a>
				</div>
			</main>

			<!-- ─── ON THIS PAGE (RIGHT) ──────────────────────────────── -->
			<aside class="hidden xl:block w-48 shrink-0 sticky top-24 h-fit border-l border-gray-100 pl-8">
				<h5 class="text-[10px] font-bold text-gray-400 uppercase tracking-widest mb-4">On this page</h5>
				<nav class="space-y-3">
					{#each sections as section}
						<button 
							onclick={() => scrollTo(section.id)}
							class="block text-[11px] font-medium leading-relaxed transition-colors text-left
								{activeSection === section.id ? 'text-indigo-600 font-bold' : 'text-gray-500 hover:text-gray-900'}"
						>
							{section.title}
						</button>
					{/each}
				</nav>
			</aside>

		</div>
	</div>
</div>

<style>
	:global(html) {
		scroll-behavior: smooth;
	}

	section {
		transition: opacity 0.5s ease;
	}

	pre {
		scrollbar-width: none;
	}
	pre::-webkit-scrollbar {
		display: none;
	}

	/* Responsive tweaks */
	@media (max-width: 1024px) {
		.sticky {
			position: static;
		}
	}
</style>
