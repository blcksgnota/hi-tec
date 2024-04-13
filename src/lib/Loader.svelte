<script>
    import {onMount} from 'svelte';
    import lottie from 'lottie-web';
    import Clock from "../assets/clock.json";
    import {gsap} from 'gsap';
    import {CustomEase} from "gsap/CustomEase";

    let loaderContainer;
    let lottieContainer;
    let animation;
    let loaderEls = [];
    let tl;
    let lastWindowWidth = window.innerWidth;
    let lastWindowHeight = window.innerHeight;
    let resizeTimeout;


    gsap.registerPlugin(CustomEase);
    function bindLoaderEl(node) {
        loaderEls = [...loaderEls, node];
    }

    const customEase = "M0,0,C0,0,0.13,0.34,0.238,0.442,0.305,0.506,0.322,0.514,0.396,0.54,0.478,0.568,0.468,0.56,0.522,0.584,0.572,0.606,0.61,0.719,0.714,0.826,0.798,0.912,1,1,1,1";
    let counter = {value: 0};
    let loaderDuration = 6;

    // If not a first time visit in this tab
    if (sessionStorage.getItem("visited") !== null) {
        loaderDuration = 4;
        counter = {value: 75};
    }
    sessionStorage.setItem("visited", "true");

    function endLoaderAnimation() {
        // Trigger any necessary actions when the loader animation ends
        // i want to make all loaderEl reduce opacity to 0 and then make it hidden
        gsap.to(loaderEls, {
            opacity: 0,
            duration: 0.5,
            delay: 1,
            onComplete: () => {
                loaderEls.forEach(el => {
                    el.style.display = "none";
                });
            }
        });

        gsap.to(loaderContainer, {
            y: '100%',
            duration: 0.8,
            // opacity: 0,
            delay: 1.25,
            // ease: CustomEase.create("custom", customEase),
            onComplete: () => {
                loaderContainer.style.opacity = 0;
                loaderContainer.style.display = "none";
            }
        });
    }

    function replayAnimations() {
        // Reset the loader elements
        loaderEls.forEach(el => {
            el.style.display = "block";
            el.style.opacity = 1;
        });

        // Reset the loader container
        loaderContainer.style.display = "flex";
        loaderContainer.style.opacity = 1;
        loaderContainer.style.transform = "translateY(0)";

        // Restart the Lottie animation
        animation.goToAndPlay(0);

        // Restart the GSAP timeline
        tl.restart();
    }

    onMount(() => {
        // Delay the animation start by 1 second (1000 milliseconds)
        setTimeout(() => {
            animation = lottie.loadAnimation({
                container: lottieContainer,
                renderer: 'svg',
                loop: false,
                autoplay: true,
                animationData: Clock,
            });

            // Set the desired duration to loaderDuration seconds
            animation.setSpeed(10 / loaderDuration);

            // Apply GSAP animation to the lottieContainer
            gsap.fromTo(
                lottieContainer,
                {scale: 1.5, opacity: 0},
                {scale: 1, opacity: 1, duration: 1.2, delay: 0.25}
            );

            // Stop the animation after loaderDuration seconds
            setTimeout(() => {
                console.log("stop");
            }, loaderDuration * 1000);
        }, 1000);

        tl = gsap.timeline({onComplete: endLoaderAnimation});

        tl.to(counter, {
            value: 100,
            duration: loaderDuration,
            ease: CustomEase.create("custom", customEase)
        });


        function handleResize() {
            if (window.innerWidth !== lastWindowWidth || window.innerHeight !== lastWindowHeight) {
                lastWindowWidth = window.innerWidth;
                lastWindowHeight = window.innerHeight;
                replayAnimations();
            }
        }

// Listen for the resize event with debounce
        window.addEventListener("resize", () => {
            clearTimeout(resizeTimeout);
            resizeTimeout = setTimeout(handleResize, 200);
        });

// Clean up the event listener on component destruction
        return () => {
            window.removeEventListener("resize", handleResize);
        };
    });
</script>


<div bind:this={loaderContainer} class="loader" style="opacity: 1; display: flex;">
    <div class="loader_bottom"
         style="transforms: translate3d(0px, 100%, 0px) scale3d(1, 1, 1) rotateX(0deg) rotateY(0deg) rotateZ(0deg) skew(0deg, 0deg); transform-style: preserve-3d; opacity: 1;">
        <div use:bindLoaderEl class="loader_logo-wrap">
            <svg class="navbar_logo" xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 136 24" fill="none">
                <path d="M17.6521 17.0978C16.6195 14.4891 13.1195 12.7717 13.1195 12.7499H23.5435C23.3369 18.9891 18.1521 23.9891 11.7717 23.9891C5.39128 23.9891 -2.67029e-05 18.8152 -2.67029e-05 12.413C-2.67029e-05 6.0108 5.27171 0.826021 11.7717 0.826021C17.75 0.826021 22.6956 5.21732 23.4456 10.9021H2.22823C8.4565 13.3695 10.3478 17.0978 10.3478 17.0978H13.2717C11.8804 14.5652 9.7391 13.2934 9.7391 13.2934C12.8804 14.1086 14.7826 17.0978 14.7826 17.0978H17.6521ZM132.641 0.467325C131.478 0.467325 130.533 1.35863 130.533 2.45646C130.533 3.55428 131.467 4.45645 132.641 4.45645C133.815 4.45645 134.728 3.58689 134.728 2.45646C134.728 1.32602 133.793 0.467325 132.641 0.467325ZM132.641 4.95645C131.196 4.95645 130.043 3.84776 130.043 2.47819C130.043 1.10863 131.196 0.0108032 132.641 0.0108032C134.087 0.0108032 135.217 1.1195 135.217 2.47819C135.217 3.83689 134.065 4.95645 132.641 4.95645ZM133.935 3.85863H133.446L132.609 2.57602H132.109V3.85863H131.674V1.07602H132.946C133.185 1.07602 133.413 1.09776 133.641 1.21732C133.87 1.33689 133.978 1.57602 133.978 1.81515C133.978 2.34776 133.576 2.56515 133.076 2.56515L133.946 3.84776L133.935 3.85863ZM132.576 2.22819C132.978 2.22819 133.533 2.31515 133.533 1.80428C133.533 1.44559 133.217 1.39124 132.859 1.39124H132.109V2.22819H132.576ZM57.3478 1.04341H50.3478L46.0108 9.34776H40.6087L44.9456 1.04341H37.9456L26.1848 23.5652H33.2065L37.7826 14.7934H43.1848L38.6087 23.5652H45.6087L57.3478 1.04341ZM67.8587 1.04341H60.7934L49.0326 23.5652H56.0978L67.8587 1.04341ZM73.2174 9.34776H66.2608L63.4239 14.7934H70.3695L73.2174 9.34776ZM84.3043 6.45646H89L91.8369 1.04341H75.6521L72.8152 6.45646H77.6195L68.6956 23.5652H75.3695L84.3043 6.45646ZM102.098 18.1738H91.7934L93.7065 14.5325H102.043L104.663 9.5108H96.3261L98.0326 6.22819H108.337L111.043 1.05428H93.8695L82.1087 23.576H99.2826L102.098 18.1738ZM118.663 14.9456C117.587 16.8152 115.761 18.6956 113.598 18.6956C110.435 18.6956 112.315 14.8586 113.489 12.5869C114.598 10.4456 117.185 5.7608 120.283 5.7608C122.652 5.7608 122.13 8.16298 121.065 10.0652L127.772 9.73906C130.25 4.13037 128.696 0.717325 122.848 0.717325C115.641 0.717325 109.663 6.09776 106.261 12.6086C102.913 19.0108 103.956 23.8912 110.837 23.8912C116.12 23.8912 121.696 20.5434 124.848 15.3478L118.663 14.9456Z"
                      fill="currentColor"></path>
            </svg>
        </div>
        <div class="loader_clock-wrap">
            <div bind:this={lottieContainer} class="loader_clock"></div>
        </div>
        <div class="loader-footer-wrap">
            <div class="text-size-medium-2 text-style-allcaps">#RunThroughTimE</div>
        </div>
        <div class="loader_elements">
            <svg use:bindLoaderEl class="loader_corner-el" width="24" xmlns="http://www.w3.org/2000/svg"
                 viewBox="0 0 24 24"
                 fill="none">
                <g clip-path="url(#clip0_2046_188)">
                    <path d="M6.66747 4H4V6.65552H6.66747V4Z" fill="currentColor"></path>
                    <path d="M12.0008 4H9.33337V6.65552H12.0008V4Z" fill="currentColor"></path>
                    <path d="M17.3341 4H14.6666V6.65552H17.3341V4Z" fill="currentColor"></path>
                    <path d="M20.0021 6.67303H17.3346V9.32855H20.0021V6.67303Z" fill="currentColor"></path>
                    <path d="M14.6681 6.67303H12.0006V9.32855H14.6681V6.67303Z" fill="currentColor"></path>
                    <path d="M9.33495 6.67303H6.66748V9.32855H9.33495V6.67303Z" fill="currentColor"></path>
                    <path d="M6.66747 9.33652H4V11.992H6.66747V9.33652Z" fill="currentColor"></path>
                    <path d="M12.0008 9.33652H9.33337V11.992H12.0008V9.33652Z" fill="currentColor"></path>
                    <path d="M17.3341 9.33652H14.6666V11.992H17.3341V9.33652Z" fill="currentColor"></path>
                    <path d="M20.0021 12.0096H17.3346V14.6651H20.0021V12.0096Z" fill="currentColor"></path>
                    <path d="M14.6681 12.0096H12.0006V14.6651H14.6681V12.0096Z" fill="currentColor"></path>
                    <path d="M9.33495 12.0096H6.66748V14.6651H9.33495V12.0096Z" fill="currentColor"></path>
                    <path d="M6.66747 14.6714H4V17.327H6.66747V14.6714Z" fill="currentColor"></path>
                    <path d="M12.0008 14.6714H9.33337V17.327H12.0008V14.6714Z" fill="currentColor"></path>
                    <path d="M17.3341 14.6714H14.6666V17.327H17.3341V14.6714Z" fill="currentColor"></path>
                    <path d="M20.0021 17.3461H17.3346V20.0016H20.0021V17.3461Z" fill="currentColor"></path>
                    <path d="M14.6681 17.3461H12.0006V20.0016H14.6681V17.3461Z" fill="currentColor"></path>
                    <path d="M9.33495 17.3461H6.66748V20.0016H9.33495V17.3461Z" fill="currentColor"></path>
                </g>
                <defs>
                    <clipPath id="clip0_2046_188">
                        <rect width="16" height="16" fill="currentColor" transform="translate(4 4)"></rect>
                    </clipPath>
                </defs>
            </svg>
            <svg use:bindLoaderEl class="loader_corner-el is-top-left" xmlns="http://www.w3.org/2000/svg"
                 width="24"
                 viewBox="0 0 24 24" fill="none">
                <g clip-path="url(#clip0_2046_164)">
                    <path d="M12 0V24" stroke="currentColor" stroke-miterlimit="10"></path>
                    <path d="M0 12H24" stroke="currentColor" stroke-miterlimit="10"></path>
                    <path d="M12 20.3428C16.6076 20.3428 20.3428 16.6076 20.3428 12C20.3428 7.39236 16.6076 3.65714 12 3.65714C7.39233 3.65714 3.6571 7.39236 3.6571 12C3.6571 16.6076 7.39233 20.3428 12 20.3428Z"
                          stroke="currentColor" stroke-miterlimit="10"></path>
                </g>
                <defs>
                    <clipPath id="clip0_2046_164">
                        <rect width="24" height="24" fill="currentColor"></rect>
                    </clipPath>
                </defs>
            </svg>
            <svg use:bindLoaderEl class="loader_corner-el is-top-right" xmlns="http://www.w3.org/2000/svg"
                 width="24"
                 viewBox="0 0 24 24" fill="none">
                <path d="M6.66861 4.00006H4.0014V6.65531H6.66861V4.00006Z" fill="currentColor"></path>
                <path d="M12.0014 4.00006H9.3342V6.65531H12.0014V4.00006Z" fill="currentColor"></path>
                <path d="M17.3342 4.00006H14.667V6.65531H17.3342V4.00006Z" fill="currentColor"></path>
                <path d="M20.0019 6.67285H17.3347V9.3281H20.0019V6.67285Z" fill="currentColor"></path>
                <path d="M14.6685 6.67285H12.0013V9.3281H14.6685V6.67285Z" fill="currentColor"></path>
                <path d="M9.33566 6.67285H6.66846V9.3281H9.33566V6.67285Z" fill="currentColor"></path>
                <path d="M6.66861 9.33603H4.0014V11.9913H6.66861V9.33603Z" fill="currentColor"></path>
                <path d="M12.0014 9.33603H9.3342V11.9913H12.0014V9.33603Z" fill="currentColor"></path>
                <path d="M17.3342 9.33603H14.667V11.9913H17.3342V9.33603Z" fill="currentColor"></path>
                <path d="M20.0019 12.0088H17.3347V14.6641H20.0019V12.0088Z" fill="currentColor"></path>
                <path d="M14.6685 12.0088H12.0013V14.6641H14.6685V12.0088Z" fill="currentColor"></path>
                <path d="M9.33566 12.0088H6.66846V14.6641H9.33566V12.0088Z" fill="currentColor"></path>
                <path d="M6.66861 14.6704H4.0014V17.3257H6.66861V14.6704Z" fill="currentColor"></path>
                <path d="M12.0014 14.6704H9.3342V17.3257H12.0014V14.6704Z" fill="currentColor"></path>
                <path d="M17.3342 14.6704H14.667V17.3257H17.3342V14.6704Z" fill="currentColor"></path>
                <path d="M20.0019 17.3448H17.3347V20.0001H20.0019V17.3448Z" fill="currentColor"></path>
                <path d="M14.6685 17.3448H12.0013V20.0001H14.6685V17.3448Z" fill="currentColor"></path>
                <path d="M9.33566 17.3448H6.66846V20.0001H9.33566V17.3448Z" fill="currentColor"></path>
            </svg>
            <svg use:bindLoaderEl class="loader_corner-el is-bottom-right" xmlns="http://www.w3.org/2000/svg"
                 width="24"
                 viewBox="0 0 24 24" fill="none">
                <g clip-path="url(#clip0_2046_160)">
                    <path d="M12 0V24" stroke="currentColor" stroke-miterlimit="10"></path>
                    <path d="M0 12H24" stroke="currentColor" stroke-miterlimit="10"></path>
                    <path d="M12 20.3428C16.6076 20.3428 20.3428 16.6076 20.3428 12C20.3428 7.39236 16.6076 3.65714 12 3.65714C7.39236 3.65714 3.65714 7.39236 3.65714 12C3.65714 16.6076 7.39236 20.3428 12 20.3428Z"
                          stroke="currentColor" stroke-miterlimit="10"></path>
                </g>
                <defs>
                    <clipPath id="clip0_2046_160">
                        <rect width="24" height="24" fill="currentColor"></rect>
                    </clipPath>
                </defs>
            </svg>
            <div use:bindLoaderEl class="loader_guide-wrap">
                <div class="loader_track-wrap"></div>
                <div class="loader_track-marker top"></div>
            </div>
            <div use:bindLoaderEl class="loader_guide-wrap is-bottom">
                <div class="loader_track-wrap"></div>
                <div class="loader_track-marker bottom"></div>
            </div>
            <div use:bindLoaderEl class="loader_guide-wrap is-left">
                <div class="loader_track-wrap is-horizontal"></div>
                <div class="loader_track-marker left"></div>
            </div>
            <div use:bindLoaderEl class="loader_guide-wrap is-right">
                <div class="loader_track-wrap is-horizontal"></div>
                <div class="loader_track-marker right"></div>
            </div>
        </div>
    </div>
</div>

<style>
    .loader {
        display: flex;
    }

    @keyframes slideX {
        0%, 100% {
            transform: translateX(185px);
        }
        50% {
            transform: translateX(calc(100vw - 9px - 185px));
        }
    }

    @keyframes slideY {
        0%, 100% {
            transform: translateY(185px);
        }
        50% {
            transform: translateY(calc(100vh - 9px - 185px));
        }
    }


    .loader_track-marker.top {
        animation: slideX 30s infinite ease-in-out;
    }

    .loader_track-marker.bottom {
        animation: slideX 20s infinite ease-in-out;
    }

    .loader_track-marker.left {
        animation: slideY 30s infinite ease-in-out;
    }

    .loader_track-marker.right {
        animation: slideY 20s infinite ease-in-out;
    }

</style>