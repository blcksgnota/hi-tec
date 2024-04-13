<script>
    import {onMount} from 'svelte';
    import {gsap} from 'gsap';
    import {activeSection} from '../store.js';

    export let menuItems;
    export let menuRefs;

    let menuOpen;
    let menuClose;
    let menuLinks;
    let t1;

    const handleNavMenu = () => {
        t1 = gsap.timeline({paused: true});

        t1.to('.menu', {
            duration: 1,
            top: 0,
            left: 0,
            opacity: 1,
            display: 'flex',
            ease: "expo.inOut",
        })
            .from('.menu__item', {
                duration: 0.8,
                y: 100,
                opacity: 0,
                stagger: 0.1,
                ease: "expo.inOut",
            }, "<0.4")
            .from('.component-header__socials span', {
                duration: 0.8,
                y: 100,
                opacity: 0,
                stagger: 0.1,
                ease: "expo.inOut",
            }, "<0.2");
    };

    onMount(() => {
        menuOpen = document.querySelector('.navbar_menu-button');
        menuClose = document.querySelector('.menu__button');
        menuLinks = document.querySelectorAll('.menu__item');

        handleNavMenu();

        menuOpen.addEventListener('click', () => t1.play());
        menuClose.addEventListener('click', () => t1.reverse());

        menuLinks.forEach(link => {
            link.addEventListener('click', () => {
                t1.reverse();
            });
        });
    });
</script>

<div data-animation="default" data-collapse="none" data-duration="400" data-easing="ease" data-easing2="ease"
     role="banner" class="navbar w-nav w-full">
    <div class="padding-global">
        <div class="container-large">
            <div class="navbar_component">
                <div class="navbar_cell">
                    <a data-color="dynamic" href="/" class="navbar_brand w-nav-brand">
                        <svg class="navbar_logo hide-mobile-landscape" xmlns="http://www.w3.org/2000/svg" width="100%"
                             viewBox="0 0 136 24" fill="none">
                            <path d="M17.6521 17.0978C16.6195 14.4891 13.1195 12.7717 13.1195 12.7499H23.5435C23.3369 18.9891 18.1521 23.9891 11.7717 23.9891C5.39128 23.9891 -2.67029e-05 18.8152 -2.67029e-05 12.413C-2.67029e-05 6.0108 5.27171 0.826021 11.7717 0.826021C17.75 0.826021 22.6956 5.21732 23.4456 10.9021H2.22823C8.4565 13.3695 10.3478 17.0978 10.3478 17.0978H13.2717C11.8804 14.5652 9.7391 13.2934 9.7391 13.2934C12.8804 14.1086 14.7826 17.0978 14.7826 17.0978H17.6521ZM132.641 0.467325C131.478 0.467325 130.533 1.35863 130.533 2.45646C130.533 3.55428 131.467 4.45645 132.641 4.45645C133.815 4.45645 134.728 3.58689 134.728 2.45646C134.728 1.32602 133.793 0.467325 132.641 0.467325ZM132.641 4.95645C131.196 4.95645 130.043 3.84776 130.043 2.47819C130.043 1.10863 131.196 0.0108032 132.641 0.0108032C134.087 0.0108032 135.217 1.1195 135.217 2.47819C135.217 3.83689 134.065 4.95645 132.641 4.95645ZM133.935 3.85863H133.446L132.609 2.57602H132.109V3.85863H131.674V1.07602H132.946C133.185 1.07602 133.413 1.09776 133.641 1.21732C133.87 1.33689 133.978 1.57602 133.978 1.81515C133.978 2.34776 133.576 2.56515 133.076 2.56515L133.946 3.84776L133.935 3.85863ZM132.576 2.22819C132.978 2.22819 133.533 2.31515 133.533 1.80428C133.533 1.44559 133.217 1.39124 132.859 1.39124H132.109V2.22819H132.576ZM57.3478 1.04341H50.3478L46.0108 9.34776H40.6087L44.9456 1.04341H37.9456L26.1848 23.5652H33.2065L37.7826 14.7934H43.1848L38.6087 23.5652H45.6087L57.3478 1.04341ZM67.8587 1.04341H60.7934L49.0326 23.5652H56.0978L67.8587 1.04341ZM73.2174 9.34776H66.2608L63.4239 14.7934H70.3695L73.2174 9.34776ZM84.3043 6.45646H89L91.8369 1.04341H75.6521L72.8152 6.45646H77.6195L68.6956 23.5652H75.3695L84.3043 6.45646ZM102.098 18.1738H91.7934L93.7065 14.5325H102.043L104.663 9.5108H96.3261L98.0326 6.22819H108.337L111.043 1.05428H93.8695L82.1087 23.576H99.2826L102.098 18.1738ZM118.663 14.9456C117.587 16.8152 115.761 18.6956 113.598 18.6956C110.435 18.6956 112.315 14.8586 113.489 12.5869C114.598 10.4456 117.185 5.7608 120.283 5.7608C122.652 5.7608 122.13 8.16298 121.065 10.0652L127.772 9.73906C130.25 4.13037 128.696 0.717325 122.848 0.717325C115.641 0.717325 109.663 6.09776 106.261 12.6086C102.913 19.0108 103.956 23.8912 110.837 23.8912C116.12 23.8912 121.696 20.5434 124.848 15.3478L118.663 14.9456Z"
                                  fill="currentColor"></path>
                        </svg>
                        <svg class="m-navbar_logo" xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 42 40"
                             fill="none">
                            <path d="M30.8728 28.0863C29.0669 23.5835 22.9455 20.6191 22.9455 20.5816H41.1765C40.8153 31.3508 31.7473 39.9812 20.5882 39.9812C9.42915 39.9812 0 31.0319 0 20C0 8.9681 9.22003 0 20.5882 0C31.0439 0 39.6937 7.57974 41.0054 17.3921H3.89713C14.7901 21.651 18.0979 28.0863 18.0979 28.0863H23.2117C20.7783 23.7148 17.0333 21.5197 17.0333 21.5197C22.5273 22.9268 25.8541 28.0863 25.8541 28.0863H30.8728Z"
                                  fill="currentColor"></path>
                        </svg>
                    </a>
                </div>
                <div class="navbar_cell align-center">
                    <nav role="navigation" class="navbar_menu w-nav-menu">
                        {#each menuItems as item}
                            <a bind:this={menuRefs[item.section]} data-section={item.section} href="#{item.section}"
                               data-theme="dynamic" class="navbar_link w-inline-block">
                                <span class="bracket" class:active={$activeSection === item.section}>[</span>
                                <span>{item.text}</span>
                                <span class="bracket" class:active={$activeSection === item.section}>]</span>
                            </a>
                        {/each}
                    </nav>
                </div>
                <div class="navbar_cell align-right">
                    <div class="text-size-small text-style-allcaps hide-mobile-landscape">#RunThroughTimE</div>
                    <button class="navbar_menu-button">
                        <div class="navbar_menu_line"></div>
                        <div class="navbar_menu_line"></div>
                    </button>
                </div>
            </div>
        </div>
    </div>
    <div class="w-nav-overlay" data-wf-ignore="" id="w-nav-overlay-0"></div>
</div>

<div class="menu">
    <div class="menu__container">
        <div class="menu__component">
            <div class="menu__header">
                <svg class="menu__logo--mobile" xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 42 40"
                     fill="none">
                    <g clip-path="url(#clip0_2149_967)">
                        <path d="M30.8728 28.0863C29.0669 23.5835 22.9455 20.6191 22.9455 20.5816H41.1765C40.8153 31.3508 31.7473 39.9812 20.5882 39.9812C9.42915 39.9812 0 31.0319 0 20C0 8.9681 9.22003 0 20.5882 0C31.0439 0 39.6937 7.57974 41.0054 17.3921H3.89713C14.7901 21.651 18.0979 28.0863 18.0979 28.0863H23.2117C20.7783 23.7148 17.0333 21.5197 17.0333 21.5197C22.5273 22.9268 25.8541 28.0863 25.8541 28.0863H30.8728Z"
                              fill="currentColor"></path>
                    </g>
                    <defs>
                        <clipPath id="clip0_2149_967">
                            <rect width="41.1765" height="40" fill="currentColor"></rect>
                        </clipPath>
                    </defs>
                </svg>
                <svg class="menu__logo" xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 136 24"
                     fill="none">
                    <g clip-path="url(#clip0_2149_696)">
                        <path d="M17.6522 17.0977C16.6196 14.489 13.1196 12.7716 13.1196 12.7499H23.5435C23.337 18.989 18.1522 23.989 11.7717 23.989C5.3913 23.989 0 18.8151 0 12.4129C0 6.01074 5.27174 0.82596 11.7717 0.82596C17.75 0.82596 22.6957 5.21726 23.4457 10.902H2.22826C8.45652 13.3694 10.3478 17.0977 10.3478 17.0977H13.2717C11.8804 14.5651 9.73913 13.2934 9.73913 13.2934C12.8804 14.1086 14.7826 17.0977 14.7826 17.0977H17.6522ZM132.641 0.467264C131.478 0.467264 130.533 1.35857 130.533 2.45639C130.533 3.55422 131.467 4.45639 132.641 4.45639C133.815 4.45639 134.728 3.58683 134.728 2.45639C134.728 1.32596 133.793 0.467264 132.641 0.467264ZM132.641 4.95639C131.196 4.95639 130.043 3.8477 130.043 2.47813C130.043 1.10857 131.196 0.0107422 132.641 0.0107422C134.087 0.0107422 135.217 1.11944 135.217 2.47813C135.217 3.83683 134.065 4.95639 132.641 4.95639ZM133.935 3.85857H133.446L132.609 2.57596H132.109V3.85857H131.674V1.07596H132.946C133.185 1.07596 133.413 1.0977 133.641 1.21726C133.87 1.33683 133.978 1.57596 133.978 1.81509C133.978 2.3477 133.576 2.56509 133.076 2.56509L133.946 3.8477L133.935 3.85857ZM132.576 2.22813C132.978 2.22813 133.533 2.31509 133.533 1.80422C133.533 1.44552 133.217 1.39118 132.859 1.39118H132.109V2.22813H132.576ZM57.3478 1.04335H50.3478L46.0109 9.3477H40.6087L44.9456 1.04335H37.9457L26.1848 23.5651H33.2065L37.7826 14.7934H43.1848L38.6087 23.5651H45.6087L57.3478 1.04335ZM67.8587 1.04335H60.7935L49.0326 23.5651H56.0978L67.8587 1.04335ZM73.2174 9.3477H66.2609L63.4239 14.7934H70.3696L73.2174 9.3477ZM84.3043 6.45639H89L91.837 1.04335H75.6522L72.8152 6.45639H77.6196L68.6956 23.5651H75.3696L84.3043 6.45639ZM102.098 18.1738H91.7935L93.7065 14.5325H102.043L104.663 9.51074H96.3261L98.0326 6.22813H108.337L111.043 1.05422H93.8696L82.1087 23.576H99.2826L102.098 18.1738ZM118.663 14.9455C117.587 16.8151 115.761 18.6955 113.598 18.6955C110.435 18.6955 112.315 14.8586 113.489 12.5868C114.598 10.4455 117.185 5.76074 120.283 5.76074C122.652 5.76074 122.13 8.16292 121.065 10.0651L127.772 9.739C130.25 4.13031 128.696 0.717264 122.848 0.717264C115.641 0.717264 109.663 6.0977 106.261 12.6086C102.913 19.0107 103.957 23.8912 110.837 23.8912C116.12 23.8912 121.696 20.5434 124.848 15.3477L118.663 14.9455Z"
                              fill="currentColor"></path>
                    </g>
                    <defs>
                        <clipPath id="clip0_2149_696">
                            <rect width="135.217" height="24" fill="currentColor"></rect>
                        </clipPath>
                    </defs>
                </svg>
                <div class="menu__col">
                    <div class="translate-x-[-25%] text-size-medium text-style-allcaps">#RunThroughTimE</div>
                </div>
                <button class="menu__button">
                    <svg xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 40 40" fill="none">
                        <circle cx="20" cy="20" r="19.5" stroke="currentColor"></circle>
                        <line x1="14.6464" y1="24.9599" x2="25.9602" y2="13.6462" stroke="currentColor"></line>
                        <line x1="14.3536" y1="13.6464" x2="25.6673" y2="24.9602" stroke="currentColor"></line>
                    </svg>
                </button>
            </div>

            <div class="menu__wrap">
                <div class="menu__items">
                    <a href="#1981" class="menu__item w-inline-block w--current">
                        <div class="menu__item-century">19</div>
                        <div class="menu__item-img-wrap">
                            <img src="/img/menu__image.webp" loading="lazy" alt="" class="menu__item-img">
                        </div>
                        <div class="menu__item-year">81</div>
                    </a>

                    <a href="#1990" class="menu__item w-inline-block w--current">
                        <div class="menu__item-century">19</div>
                        <div class="menu__item-img-wrap">
                            <img src="/img/menu__image.webp" loading="lazy" alt="" class="menu__item-img">
                        </div>
                        <div class="menu__item-year">90</div>
                    </a>

                    <a href="#1992" class="menu__item w-inline-block w--current">
                        <div class="menu__item-century">19</div>
                        <div class="menu__item-img-wrap">
                            <img src="/img/menu__image.webp" loading="lazy" alt="" class="menu__item-img">
                        </div>
                        <div class="menu__item-year">92</div>
                    </a>

                    <a href="#2007" class="menu__item w-inline-block w--current">
                        <div class="menu__item-century">20</div>
                        <div class="menu__item-img-wrap">
                            <img src="/img/menu__image.webp" loading="lazy" alt="" class="menu__item-img">
                        </div>
                        <div class="menu__item-year">07</div>
                    </a>

                    <a href="#2021" class="menu__item w-inline-block w--current">
                        <div class="menu__item-century">20</div>
                        <div class="menu__item-img-wrap">
                            <img src="/img/menu__image.webp" loading="lazy" alt="" class="menu__item-img">
                        </div>
                        <div class="menu__item-year">21</div>
                    </a>

                    <a href="#shop" class="menu__item w-inline-block">
                        <div class="menu__item-century">20</div>
                        <div class="menu__item-img-wrap">
                            <img src="/img/menu__image.webp" loading="lazy" alt="" class="menu__item-img">
                        </div>
                        <div class="menu__item-year">24</div>
                    </a>
                </div>
                <div class="menu__content">
                    <div class="menu__content-text">we’ve been running since 1974. 5 pairs in particular, have shaped
                        our history and taken us global. explore the stories of these 5 pairs by clicking on one of the
                        years above.
                    </div>
                </div>
                <div class="menu__footer">
                    <div class="text-size-small">OUR HERITAGE</div>
                    <div class="text-size-small">OUR FUTURE</div>
                </div>
                <div class="menu__points">
                    <svg class="menu__point" xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 24 72"
                         fill="none">
                        <g clip-path="url(#clip0_4064_4)">
                            <path d="M12 24V48" stroke="currentColor" stroke-miterlimit="10"></path>
                            <path d="M0 36H24" stroke="currentColor" stroke-miterlimit="10"></path>
                            <path d="M12.0001 44.3429C16.6077 44.3429 20.3429 40.6077 20.3429 36.0001C20.3429 31.3925 16.6077 27.6572 12.0001 27.6572C7.39245 27.6572 3.65723 31.3925 3.65723 36.0001C3.65723 40.6077 7.39245 44.3429 12.0001 44.3429Z"
                                  stroke="currentColor" stroke-miterlimit="10"></path>
                        </g>
                        <rect x="8" width="8" height="8" rx="2" fill="currentColor"></rect>
                        <rect x="8" y="64" width="8" height="8" rx="2" fill="currentColor"></rect>
                        <defs>
                            <clipPath id="clip0_4064_4">
                                <rect width="24" height="24" fill="currentColor" transform="translate(0 24)"></rect>
                            </clipPath>
                        </defs>
                    </svg>
                    <svg class="menu__point" xmlns="http://www.w3.org/2000/svg" width="100%" viewBox="0 0 24 72"
                         fill="none">
                        <g clip-path="url(#clip0_4064_4)">
                            <path d="M12 24V48" stroke="currentColor" stroke-miterlimit="10"></path>
                            <path d="M0 36H24" stroke="currentColor" stroke-miterlimit="10"></path>
                            <path d="M12.0001 44.3429C16.6077 44.3429 20.3429 40.6077 20.3429 36.0001C20.3429 31.3925 16.6077 27.6572 12.0001 27.6572C7.39245 27.6572 3.65723 31.3925 3.65723 36.0001C3.65723 40.6077 7.39245 44.3429 12.0001 44.3429Z"
                                  stroke="currentColor" stroke-miterlimit="10"></path>
                        </g>
                        <rect x="8" width="8" height="8" rx="2" fill="currentColor"></rect>
                        <rect x="8" y="64" width="8" height="8" rx="2" fill="currentColor"></rect>
                        <defs>
                            <clipPath id="clip0_4064_4">
                                <rect width="24" height="24" fill="currentColor" transform="translate(0 24)"></rect>
                            </clipPath>
                        </defs>
                    </svg>
                </div>
            </div>
        </div>
    </div>
</div>