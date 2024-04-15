<script>
    import {Application} from "@splinetool/runtime";
    import {onMount} from "svelte";

    let canvasRef;
    export let year;
    export let yearHighlight;
    export let headline;
    export let subheadline;

    // onMount(() => {
    //     const app = new Application(canvasRef);
    //     app.load(`/splines/${year}/scene.splinecode`);
    // });

</script>


<section id={year} class="section_hero">
    <div class="padding-global">
        <div class="w-layout-blockcontainer container-large w-container">
            <div class="row cols-24 desktop">
                <div class="col col-lg-4">
                    <div class="hero__content justify-between">
                        <div class="hero__first-image_wrap">
                            <img src="{`/img/${year}/hero-image.webp`}" loading="eager" width="210" alt="" class="hero__first-image">
                        </div>
                        <div class="hero__date-wrap">
                            <div data-w-id="f9720988-5e6e-2b43-8298-2be51d7de46e" class="hero_date-wrap desktop">
                                <h1 class="heading-style-h1 color-inherit">
                                    <div class="hero_heading-block is-year"><span class="date_heading-top">{year}</span>
                                    </div>
                                </h1>
                                <div class="text-style-allcaps">
                                    <div class="date_heading-block">
                                        <div class="date_heading-bottom text-size-small">{yearHighlight}</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col col-lg-16">
                    <div class="hero__content">
                        <div class="hero__tittle-wrap">
                            <h1 class="heading-style-h1 text-align-center">
                                {@html headline.replace(/\n/g, '<br>')}
                            </h1>
                        </div>
                        <div class="hero__spline-wrap spline">
                            <div class="hero__spline-placeholder spline">
                                <canvas bind:this={canvasRef} id={`3d-${year}`} style="display: block; width: 100%; height: 100%;"></canvas>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col col-lg-4 last">
                    <div class="hero__content justify-end">
                        <div class="hero__paragraph-wrap">
                            <div class="hero__paragraph text-size-medium">
                                <div>{subheadline}</div>
                            </div>
                        </div>
                        <div class="hero__lightbox-wrap">
                            <a href="" class="hero__lightbox hide w-inline-block w-lightbox" aria-label="open lightbox" aria-haspopup="dialog">
                                <img src="https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/65f99d2ed541111831d9ddb6_1090_thumbnail.webp" loading="lazy" sizes="100vw"
                                srcset="https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/65f99d2ed541111831d9ddb6_1090_thumbnail-p-500.webp 500w, https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/65f99d2ed541111831d9ddb6_1090_thumbnail-p-800.webp 800w, https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/65f99d2ed541111831d9ddb6_1090_thumbnail.webp 840w"
                                alt="" class="hero__lightbox-img">
                            <script type="application/json" class="w-json">{
                                "items"
                            :
                                [
                                    {
                                        "url": "https://www.youtube.com/watch?v=vGIlNRS31qM",
                                        "originalUrl": "https://www.youtube.com/watch?v=vGIlNRS31qM",
                                        "width": 940,
                                        "height": 528,
                                        "thumbnailUrl": "https://i.ytimg.com/vi/vGIlNRS31qM/hqdefault.jpg",
                                        "html": "<iframe class=\"embedly-embed\" src=\"//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FvGIlNRS31qM%3Ffeature%3Doembed&display_name=YouTube&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DvGIlNRS31qM&image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FvGIlNRS31qM%2Fhqdefault.jpg&key=96f1f04c5f4143bcb0f2e68c87d65feb&type=text%2Fhtml&schema=youtube\" width=\"940\" height=\"528\" scrolling=\"no\" title=\"YouTube embed\" frameborder=\"0\" allow=\"autoplay; fullscreen; encrypted-media; picture-in-picture;\" allowfullscreen=\"true\"></iframe>",
                                        "type": "video"
                                    }
                                ],
                                    "group"
                            :
                                ""
                            }
                            </script>
                        </a></div>
                    </div>
                </div>
            </div>
            <div class="u-img-cover">
                <img src={`/img/${year}/background.webp`} loading="lazy" width="1920" sizes="(max-width: 991px) 100vw, 1190.390625px" alt=""  class="hero_image">
            </div>
            <div class="m-hero__wrap">
                <div class="hero__heading-wrap">
                    <h1 class="heading-style-h1">{headline}</h1>
                    <div class="text-size-regular">
                        {subheadline}
                    </div>
                </div>
                <div class="m-hero__spline-wrap">
                    <div class="hero__spline-placeholder spline">
                        <canvas bind:this={canvasRef} id={`3d-${year}`} style="display: block; width: 100%; height: 100%;"></canvas>
                    </div>
                    <div class="m-hero__year-wrap">
                        <div data-w-id="f9720988-5e6e-2b43-8298-2be51d7de46e" class="hero_date-wrap desktop">
                            <h1 class="heading-style-h1 color-inherit">
                                <div class="hero_heading-block is-year"><span class="date_heading-top">{year}</span></div>
                            </h1>
                            <div class="text-style-allcaps">
                                <div class="date_heading-block">
                                    <div class="date_heading-bottom text-size-medium">{yearHighlight}</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="hero__lightbox-wrap">
                    <a href="#" class="hero__lightbox hide w-inline-block w-lightbox" aria-label="open lightbox" aria-haspopup="dialog">
                        <img src="https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/65f99d2ed541111831d9ddb6_1090_thumbnail.webp" loading="lazy" width="420" sizes="100vw" alt=""
                        srcset="https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/65f99d2ed541111831d9ddb6_1090_thumbnail-p-500.webp 500w, https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/65f99d2ed541111831d9ddb6_1090_thumbnail-p-800.webp 800w, https://assets-global.website-files.com/65e6ea25b58cfc23004a325a/65f99d2ed541111831d9ddb6_1090_thumbnail.webp 840w"
                        class="hero__lightbox-img">
                    <script type="application/json" class="w-json">
                    {
                        "items"
                    :
                        [
                            {
                                "url": "https://www.youtube.com/watch?v=vGIlNRS31qM",
                                "originalUrl": "https://www.youtube.com/watch?v=vGIlNRS31qM",
                                "width": 940,
                                "height": 528,
                                "thumbnailUrl": "https://i.ytimg.com/vi/vGIlNRS31qM/hqdefault.jpg",
                                "html": "<iframe class=\"embedly-embed\" src=\"//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FvGIlNRS31qM%3Ffeature%3Doembed&display_name=YouTube&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DvGIlNRS31qM&image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FvGIlNRS31qM%2Fhqdefault.jpg&key=96f1f04c5f4143bcb0f2e68c87d65feb&type=text%2Fhtml&schema=youtube\" width=\"940\" height=\"528\" scrolling=\"no\" title=\"YouTube embed\" frameborder=\"0\" allow=\"autoplay; fullscreen; encrypted-media; picture-in-picture;\" allowfullscreen=\"true\"></iframe>",
                                "type": "video"
                            }
                        ],
                            "group"
                    :
                        ""
                    }
                    </script>
                </a></div>
            </div>
        </div>
    </div>
</section>