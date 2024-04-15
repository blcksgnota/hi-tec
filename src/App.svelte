<script>
    import "../src/app.css";
    import "../src/webflow.css";
    import {onMount} from "svelte";
    import {Application} from "@splinetool/runtime";
    import {gsap} from "gsap";
    import {ScrollTrigger} from "gsap/ScrollTrigger";
    import {ScrollToPlugin} from "gsap/ScrollToPlugin";
    import {activeSection} from "./store.js";
    import Loader from "./lib/Loader.svelte";
    import Landing from "./lib/sections/landing.svelte";
    import Navbar from "./lib/navbar.svelte";
    import Footer from "./lib/footer.svelte";
    import Overview from "./lib/sections/overview.svelte";
    import Squash from "./lib/sections/squash.svelte";
    import Peak from "./lib/sections/peak.svelte";
    import Race from "./lib/sections/race.svelte";
    import Nick from "./lib/sections/nick.svelte";
    import Shop from "./lib/sections/shop.svelte";

    let foc_container;
    let shopWrapper;
    let menuRefs = {};

    const data = [
        {
            year: "1981",
            yearHighlight: "HI-TEC Silver SHADOW",
            headline: "The London marathon\n" + "started in these",
            subheadline: "When the starting pistol fired in Greenwich park to signal the first ever London Marathon, 23% of runners took their first step wearing Hi-Tec Silver Shadows.",
        },
        {
            year: "1990",
            yearHighlight: "HI-TEC Altitude IV",
            headline: "This shoe hiked\n" + "50 peaks",
            subheadline: "Imagine climbing the highest peak in every state in the USA. Now, do it in only 101 days. That’s what mountaineer Adrian Crane did while wearing the Hi-Tec Altitude IVs. Through white-outs, mud slides, and past creatures and crevices best left undisturbed, the Altitude IVs kept going.",
        },
        {
            year: "1992",
            yearHighlight: "HI-TEC Kings Cup Boots",
            headline: "An Irish Red Devil\n" + "Wore These.",
            subheadline: "In the first season of the English Premier League, the Kings Cup Boots were worn by 52 players. One particular Irishman was especially “Keane on Hi-Tec®”. The boot would go on to be a popular choice for footballers everywhere as the Irishman lifted the first Premier League trophy.",
        },
        {
            year: "2007",
            yearHighlight: "HI-TEC Altitude IV",
            headline: "These shoes won 3\n" + "major championships",
            subheadline: "No European-born golfer had won the PGA tour in 78 years. Until an Irishman named Padraig put these on. He went on to win 2 consecutive open championships. Before taking the biggest prize of them all in 2008, the PGA championship.",
        },
        {
            year: "2021",
            yearHighlight: "HI-TEC Altitude IV",
            headline: "These shoes continue the\n" + "long walk to freedom",
            subheadline: "Nelson Mandela, had a favourite pair of shoes: a modest pair of Hi-Tec tennis shoes. We rereleased these shoes as the “Freedom 67s”, with every sale contributing to the Nelson Mandela Children’s Fund. These shoes continue the necessary work Madiba started.",
        }
    ]
    const splines = [
        {id: "3d-1981", url: "/splines/1981/scene.splinecode", objectName: "Shoe_Spline_01"},
        {id: "3d-1990", url: "/splines/1990/scene.splinecode", objectName: "Shoe_Spline_01"},
        {id: "3d-1992", url: "/splines/1992/scene.splinecode" , objectName: "Shoe_Spline_01"},
        {id: "3d-2007", url: "/splines/2007/scene.splinecode" , objectName: "Shoe_Spline_01"},
        {id: "3d-2021", url: "/splines/2021/scene.splinecode", objectName: "MD_Freedom_Shoe_Spline_01"},
    ];

    const menuItems = [
        {section: "1981", text: "1981"},
        {section: "1990", text: "1990"},
        {section: "1992", text: "1992"},
        {section: "2007", text: "2007"},
        {section: "2021", text: "2021"},
        {section: "shop", text: "Shop New Range"},
    ];

    // function loadSpline(canvasId, splineUrl) {
    //     const canvas = document.getElementById(canvasId);
    //     const app = new Application(canvas);
    //     app.load(splineUrl);
    // }

    let loadedScenes = [];

    function loadSpline(canvasId, splineUrl, objectName) {
        const canvas = document.getElementById(canvasId);
        const app = new Application(canvas);

        return new Promise((resolve, reject) => {
            app.load(splineUrl)
                .then((loadedScene) => {
                    const splineLoaded = () => {
                        const object = app.scene.findObjectByName(objectName);

                        if (object) {
                            canvas.addEventListener('mousemove', (event) => {
                                const canvasRect = canvas.getBoundingClientRect();
                                const mouseX = event.clientX - canvasRect.left;
                                const canvasWidth = canvasRect.width;
                                const mousePercentage = (mouseX / canvasWidth) * 2 - 1; // Range: -1 to 1

                                const rotationY = mousePercentage * 2; // Adjust the multiplier as needed
                                const rotationZ = 0;

                                object.rotation.y = rotationY;
                                object.rotation.z = rotationZ;
                            });

                            loadedScenes = [...loadedScenes, loadedScene];
                            resolve();
                        } else {
                            console.warn(`Object "${objectName}" not found in the spline.`);
                            reject(`Object "${objectName}" not found in the spline.`);
                        }
                    };

                    if (app.scene) {
                        splineLoaded();
                    } else {
                        app.addEventListener('load', splineLoaded);
                    }
                })
                .catch((error) => {
                    console.error('Error loading spline:', error);
                    reject(error);
                });
        });
    }


    onMount(() => {
        // Promise.all(splines.map(({ id, url, objectName }) => loadSpline(id, url, objectName)))
        //     .then(() => {
        //         console.log('All splines loaded:', loadedScenes);
        //     })
        //     .catch((error) => {
        //         console.error('Error loading splines:', error);
        //     });
        // splines.forEach(({id, url}) => loadSpline(id, url));

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
                let relatedLink = menuRefs[section.id];

                ScrollTrigger.create({
                    trigger: section,
                    start: "left center",
                    end: "right center",
                    containerAnimation: scrollTween,
                    onToggle: (self) => {
                        if (self.isActive) {
                            activeSection.set(section.id);
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
                start: "top center",  // Trigger 5% past the top of the viewport
                end: "bottom center",      // Trigger at the bottom of the viewport
                onToggle: (self) => {
                    if (self.isActive) {
                        activeSection.set("shop");
                        toggleColor("#494949");
                        currentView = "shop";
                    }
                },
                onLeaveBack: () => {
                    currentView = "";
                    activeSection.set("2021");
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

    console.log($activeSection);
</script>

<svelte:head>
    <script
            async
            src="https://ga.jspm.io/npm:es-module-shims@1.9.0/dist/es-module-shims.js"
    ></script>
</svelte:head>

<Navbar {menuItems} {menuRefs}/>
<!--<Loader/>-->
<div class="page-wrapper">
    <div class="foc_container flex flex-col md:flex-row flex-nowrap" bind:this={foc_container}>
        <Landing/>
        <Squash/>
        <Overview year={data[0].year} yearHighlight={data[0].yearHighlight} headline={data[0].headline}
                  subheadline="{data[0].subheadline}"/>
        <!-- TODO: add magnum section-->

        <Overview year={data[1].year} yearHighlight={data[1].yearHighlight} headline={data[1].headline}
                  subheadline="{data[1].subheadline}"/>

        <Peak/>

        <Overview year={data[2].year} yearHighlight={data[2].yearHighlight} headline={data[2].headline}
                  subheadline="{data[2].subheadline}"/>

        <Race/>

        <Overview year={data[3].year} yearHighlight={data[3].yearHighlight} headline={data[3].headline}
                  subheadline="{data[3].subheadline}"/>

        <Nick/>
        <Overview year={data[4].year} yearHighlight={data[4].yearHighlight} headline={data[4].headline}
                  subheadline="{data[4].subheadline}"/>
    </div>

    <div bind:this={shopWrapper}>
        <Shop shopLink="shop"/>
    </div>
</div>
<Footer/>
