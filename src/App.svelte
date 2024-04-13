<script>
    import "../src/app.css";
    import { onMount } from "svelte";
    import { Application } from '@splinetool/runtime';
    import { gsap } from "gsap";
    import { ScrollTrigger } from "gsap/ScrollTrigger";
    import { ScrollToPlugin } from "gsap/ScrollToPlugin";

    let canvas;
    let scale;
    let foc_container;
    let shopWrapper;
    let shopLink;
    let twenty21Link;

    const splines = [
        { id: '3d-1981', url: 'https://prod.spline.design/72BmcF8KyGZm6gPr/scene.splinecode' },
        { id: '3d-1990', url: 'https://prod.spline.design/3XHgeETBd5dCmLLH/scene.splinecode' },
        { id: '3d-1992', url: 'https://prod.spline.design/FkBQ62fJMTBuJJtn/scene.splinecode' },
        { id: '3d-2007', url: 'https://prod.spline.design/ttzlOxVtoas2fZSY/scene.splinecode' },
        { id: '3d-2021', url: 'https://prod.spline.design/HSNPExzkavITuAvu/scene.splinecode' },
    ];

    const menuItems = [
        { section: "1981", text: "1981" },
        { section: "1990", text: "1990" },
        { section: "1992", text: "1992" },
        { section: "2007", text: "2007" },
        { section: "2021", text: "2021" },
        { section: "shop", text: "Shop" },
    ];

    function loadSpline(canvasId, splineUrl) {
        const canvas = document.getElementById(canvasId);
        const app = new Application(canvas);
        app.load(splineUrl);
    }

    onMount(() => {
        splines.forEach(({ id, url }) => loadSpline(id, url));

        const root_theme = document.querySelector(":root");
        const sections = document.querySelectorAll(".section_track");

        function toggleColor(color) {
            root_theme.style.setProperty("--main-color", color);
        }

        // Check if the device is desktop
        if (window.innerWidth >= 1024) {
            const foc_sections = gsap.utils.toArray(".foc_container section");

            let twenty21TargetScrollX = 0;
            let totalWidth = 0;
            let currentView = "";
            foc_sections.forEach((section) => {
                totalWidth += section.offsetWidth;
            });
            foc_container.style.width = `${totalWidth}px`;

            // Setup navigation
            const navbarLinks = document.querySelectorAll(".navbar_menu a");
            const menuLinks = document.querySelectorAll(".menu__item");
            gsap.registerPlugin(ScrollToPlugin, ScrollTrigger);

            // Create a horizontal scrolling tween for the sections container
            const scrollTween = gsap.to(foc_container, {
                x: () => -(totalWidth - window.innerWidth),
                ease: "none",
                scrollTrigger: {
                    trigger: foc_container,
                    pin: true,
                    start: "top top",
                    scrub: 1,
                    invalidateOnRefresh: true,
                    end: () => "+=" + (totalWidth - window.innerWidth),
                },
            });

            // Create a ScrollTrigger for each section
            foc_sections.forEach((section, i) => {
                let relatedLink = document.querySelector(
                    `.navbar a[href="#${section.id}"]`,
                );
                ScrollTrigger.create({
                    trigger: section,
                    start: "left center",
                    end: "right center",
                    containerAnimation: scrollTween,
                    onToggle: (self) => {
                        if (self.isActive) {
                            if (relatedLink) {
                                navbarLinks.forEach((link) => {
                                    link.classList.remove("active");
                                });
                                toggleColor("#494949");
                                relatedLink?.classList.add("active");
                            } else if (i === 0) {
                                navbarLinks.forEach((link) => {
                                    link.classList.remove("active");
                                });
                                toggleColor("#494949");
                            } else {
                                navbarLinks.forEach((link) => {
                                    link.classList.remove("active");
                                });
                                toggleColor("white");
                            }
                        }
                    },
                });
            });

            // shop section trigger
            ScrollTrigger.create({
                trigger: shopWrapper,
                start: "top center",
                end: "bottom center",
                onToggle: (self) => {
                    if (self.isActive) {
                        navbarLinks.forEach((link) => link.classList.remove("active"));
                        shopLink?.classList?.add("active");
                        toggleColor("#494949");
                        currentView = "shop";
                    }
                },
                onLeaveBack: () => {
                    currentView = "";
                    navbarLinks.forEach((link) => link.classList.remove("active"));
                    twenty21Link?.classList?.add("active");
                    toggleColor("#494949");
                },
            });

            // Scroll to section on navbar link click
            [...navbarLinks, ...menuLinks].forEach((anchor) => {
                anchor.addEventListener("click", function (e) {
                    e.preventDefault();
                    let targetID = this.getAttribute("href").replace("#", "");
                    let targetElem = document.getElementById(targetID);
                    if (targetElem) {
                        if (foc_container.contains(targetElem)) {
                            let targetScrollX = 0;
                            for (let elem of foc_sections) {
                                if (elem.id === targetID) {
                                    if (targetID === "2021") {
                                        twenty21TargetScrollX = targetScrollX;
                                    }
                                    break;
                                }
                                targetScrollX += elem.offsetWidth;
                            }
                            //if we are navigating from shop section we need to fix the jaggy animation in the UI
                            // by scrolling to 2021 first then finally scroll to the intended section
                            if (currentView === "shop" && targetID !== "2021") {
                                currentView = "";
                                gsap.to(window, {
                                    scrollTo: {
                                        y: twenty21TargetScrollX,
                                        autoKill: false,
                                    },
                                    duration: 1,
                                });
                                setTimeout(() => {
                                    gsap.to(window, {
                                        scrollTo: {
                                            y: targetScrollX,
                                            autoKill: false,
                                        },
                                        duration: 1,
                                    });
                                }, 1600);
                            } else {
                                gsap.to(window, {
                                    scrollTo: {
                                        y: targetScrollX,
                                        autoKill: false,
                                    },
                                    duration: 1.5,
                                });
                            }
                        } else {
                            // Scroll to the shop wrapper if it's not inside foc_container
                            gsap.to(window, {
                                scrollTo: {
                                    y: targetElem.offsetTop,
                                    autoKill: false,
                                },
                                duration: 1,
                            });
                        }
                    }
                });
            });
        }
    });
</script>

<svelte:head>
    <link href="https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/css/hi-tec-staging.webflow.f0266fd4f.css" rel="stylesheet" type="text/css">
    <script async src="https://ga.jspm.io/npm:es-module-shims@1.9.0/dist/es-module-shims.js"></script>
</svelte:head>

<header>
    <div class="navbar_menu">
        {#each menuItems as item}
            <a data-section={item.section} href="#{item.section}" data-theme="dynamic" class="navbar_link w-inline-block">
                <span class="bracket">[</span>
                <span>{item.text}</span>
                <span class="bracket">]</span>
            </a>
        {/each}
    </div>
</header>
<main class="bg-amber-50 overflow-clip">
    <div class="foc_container" bind:this={foc_container}>
        <section id="" class="section_hero hero">
            Hero
        </section>

        <section class="w-[200vw] bg-gray-950 text-white section_track">
            <h1>Text color white and this section is long not just small long but big long</h1>
        </section>

        <section id="1981" class="section_hero">
            <h1>1981</h1>
            <canvas id="3d-1981"></canvas>
        </section>

        <section class="w-[200vw] bg-gray-950 text-white section_track">
            <h1>Text color white and this section is long not just small long but big long</h1>
        </section>

        <section id="1990" class="section_hero">
            <h1>1990</h1>
            <canvas id="3d-1990"></canvas>
        </section>

        <section class="w-[200vw] bg-gray-950 text-white section_track">
            <h1>Text color white and this section is long not just small long but big long</h1>
        </section>

        <section id="1992" class="section_hero">
            <h1>1992</h1>
            <canvas id="3d-1992"></canvas>
        </section>

        <section class="w-[200vw] bg-gray-950 text-white section_track">
            <h1>Text color white and this section is long not just small long but big long</h1>
        </section>

        <section id="2007" class="section_hero">
            <h1>2007</h1>
            <canvas id="3d-2007"></canvas>
        </section>

        <section class="w-[200vw] bg-gray-950 text-white section_track">
            <h1>Text color white and this section is long not just small long but big long</h1>
        </section>

        <section id="2021" class="section_hero">
            <h1>2021</h1>
            <canvas id="3d-2021"></canvas>
        </section>
    </div>

    <section id="shop" class="bg-white h-[500vh] justify-start" bind:this={shopWrapper}>
        Shop
    </section>
</main>