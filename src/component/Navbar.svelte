<script>
    import { onMount } from "svelte";
    import { page } from "$app/stores";
    import HamburgerIcon from "./HamburgerIcon.svelte";

    let overay_active = false;
    let nav_active = false;
    let dropdown_active = false;
    $: scrollY = 0;
    let scrollY_copy = 0;

    let menu = [
        { path: "", name: "Home" },
        { path: "#hero", name: "부천셔츠룸" },
        { path: "#a1", name: "가격안내" },
        { path: "#a2", name: "셔츠룸이란" },
        { path: "#a3", name: "오시는길" },
        { path: "#a4", name: "게시판" },
    ];

    function toggleMenu() {
        scrollY_copy = scrollY;
        overay_active = !overay_active;
        dropdown_active = !dropdown_active;
    }

    function toggleNav() {
        nav_active = !nav_active;
    }

    function scroll() {
        console.log("ddjio", scrollY, scrollY_copy);
        scrollY <= scrollY_copy ? (nav_active = true) : (nav_active = false);
        scrollY_copy = scrollY;
    }

    onMount(() => {
        toggleNav();
    });
</script>

<svelte:window bind:scrollY on:scroll={scroll} />

<div class="relative">
    <div class="fixed w-full min-h-fit duration-1000 z-10 nav" class:nav_active>
        <div class="flex justify-between mx-auto max-w-screen-lg text-white p-4">
            <a href="/">
                <div class="text-3xl font-bold">
                    부천 <span class="text-red-400">노래방</span>
                </div>
            </a>
            <button class="h-icon" on:click={toggleMenu}><HamburgerIcon isActive={dropdown_active}/></button>
        </div>
    </div>

    <!-- menu -->
    <div
        class="fixed flex flex-col w-5/6 max-w-sm h-screen py-6 px-6 left-[-100vw] bg-gray-600 duration-1000 z-10"
        class:dropdown_active
    >
        <div class="flex items-center mb-8">
            <a href="/" class="text-3xl font-bold mr-auto"> 
                <div class="text-3xl font-bold text-white">
                    부천 <span class="text-red-400">노래방</span>
                </div> 
            </a>
            <button on:click={toggleMenu}>
                <svg
                    class="h-6 w-6 text-gray-400 cursor-pointer hover:text-gray-500"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        stroke-width="2"
                        d="M6 18L18 6M6 6l12 12"
                    ></path>
                </svg>
            </button>
        </div>
        <div>
            <ul>
                {#each menu as item}
                    <li class="mb-1">
                        <a
                            href={item.path}
                            class={$page.url.hash === item.path
                                ? "block p-4 text-sm font-semibold bg-blue-50 text-blue-600 rounded"
                                : "block p-4 text-sm font-semibold text-gray-400 hover:bg-blue-50 hover:text-blue-600 rounded"}
                            on:click={() => {
                                toggleMenu();
                            }}
                        >
                            {item.name}
                        </a>
                    </li>
                {/each}
            </ul>
        </div>
        <div class="mt-auto">
            <div class="pt-6">
                <a
                    class="block px-4 py-3 mb-3 text-xs text-center font-semibold bg-green-500 hover:bg-green-600 rounded-xl"
                    href="tel:01066866686">전화하기</a
                >
                <a
                    class="block px-4 py-3 mb-2 text-xs text-center text-white font-semibold bg-yellow-500 hover:bg-yellow-600 rounded-xl"
                    href="http://qr.kakao.com/talk/gMdxTKOrP3dc3VOI0JX8zNjBV5Q-">카카오톡</a
                >
            </div>
            <p class="my-4 text-xs text-center text-gray-400">
                <span>Designed by </span>
            </p>
        </div>
    </div>
    <div
        class="fixed h-full w-full duration-1000 pointer-events-none"
        class:overay_active
    ></div>
</div>

<!-- <div class="h-[25px]"></div> -->
<style>
    .dropdown_active {
        left: 0;
    }
    .overay_active {
        background-color: black;
        opacity: 0.5;
        pointer-events: auto;
    }
    .nav {
        top: -25px;
    }
    .nav_active {
        background-color: rgba(0, 0, 0, 1);
        top: 0px;
    }
</style>
