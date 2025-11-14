<script lang="ts">
  import ProfilePicture from './profile-picture.svelte';
  import { page } from '$app/stores';
  import showHamburgerMenu from '$lib/stores/show-hamburger-menu';
  import HamburgerMenu from './hamburger-menu/menu.svelte';
  import { onMount } from 'svelte';
  import { showHideOverflowY } from '../util/hide-overflow';

  export let username: string | undefined;

  const navItems = [
    { name: 'Docs', path: '/docs' },
    { name: 'App', path: '/app' }
  ];

  onMount(() => {
    const query = window.matchMedia('(min-width: 768px)');

    const handleTabletChange = (e: MediaQueryListEvent) => {
      if (e.matches) {
        showHamburgerMenu.set(false);
        showHideOverflowY(false);
      }
    };

    query.addEventListener('change', handleTabletChange);
  });
</script>

<header class="my-6 flex flex-wrap items-center justify-between px-2 md:px-6 animate-fade-in">
  <a href="/" class="flex items-center gap-2 hover:opacity-80 transition">
    <img
      src="/images/hubber.png"
      class="w-8 md:w-12 object-contain drop-shadow-sm"
      alt="hubber logo"
    />
  </a>

  <!-- Mobile Menu Button -->
  <button
    class="lg:hidden flex items-center p-3 text-neutral-900 dark:text-white hover:scale-110 transition"
    on:click={() => showHamburgerMenu.set(true)}
    data-test-id="hamburger-btn"
    aria-label="Open navigation menu"
  >
    <svg class="h-5 w-5 block dark:hidden" viewBox="0 0 20 20">
      <path d="M0 3h20v2H0zm0 6h20v2H0zm0 6h20v2H0z" />
    </svg>
    <svg class="h-5 w-5 hidden dark:block" viewBox="0 0 20 20">
      <path d="M0 3h20v2H0zm0 6h20v2H0zm0 6h20v2H0z" />
    </svg>
  </button>

  <!-- Desktop Navigation -->
  <nav class="hidden lg:flex items-center gap-6">
    {#each navItems as item}
      <a
        href={item.path}
        class="default-transition text-skin-text-highlight hover:underline decoration-transparent font-medium tracking-wide"
        class:active={$page.url.pathname.startsWith(item.path)}
      >
        {item.name}
      </a>
    {/each}

    {#if username}
      <ProfilePicture {username} />
    {:else}
      <a
        href="/login"
        data-test-id="login-btn-lg"
        class="relative rounded-xl px-5 py-2 bg-skin-primary text-white shadow-md hover:shadow-lg transition active:scale-95"
      >
        Login
      </a>
    {/if}
  </nav>

  <!-- Hamburger Drawer -->
  <HamburgerMenu {navItems} {username} />
</header>

<style lang="postcss">
  .active {
    @apply underline decoration-inherit font-semibold;
  }

  @keyframes fade-in {
    from {
      opacity: 0;
      transform: translateY(-10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .animate-fade-in {
    animation: fade-in 0.4s ease-out;
  }
</style>