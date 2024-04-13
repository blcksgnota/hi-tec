<script>
    import "../src/app.css";
    import { onMount} from "svelte";
    import { Application } from '@splinetool/runtime';

    let canvas;
    let scale;

    const splines = [
        { id: '3d-1981', url: 'https://prod.spline.design/72BmcF8KyGZm6gPr/scene.splinecode' },
        { id: '3d-1990', url: 'https://prod.spline.design/3XHgeETBd5dCmLLH/scene.splinecode' },
        { id: '3d-1992', url: 'https://prod.spline.design/FkBQ62fJMTBuJJtn/scene.splinecode' },
        { id: '3d-2007', url: 'https://prod.spline.design/ttzlOxVtoas2fZSY/scene.splinecode' },
        { id: '3d-2021', url: 'https://prod.spline.design/HSNPExzkavITuAvu/scene.splinecode' },
    ];
    function loadSpline(canvasId, splineUrl) {
        const canvas = document.getElementById(canvasId);
        const app = new Application(canvas);
        app.load(splineUrl);
    }
    onMount(() => {
        splines.forEach(({ id, url }) => loadSpline(id, url));
    })
</script>
<svelte:head>
    <script async src="https://ga.jspm.io/npm:es-module-shims@1.9.0/dist/es-module-shims.js"></script>
</svelte:head>
<main class="bg-amber-50">
    <div>
        <section>
            Hero
        </section>

        <section>
            <h1>1981</h1>
            <canvas id="3d-1981"></canvas>
        </section>

        <section>
            <h1>1990</h1>
            <canvas id="3d-1990"></canvas>
        </section>

        <section>
            <h1>1992</h1>
            <canvas id="3d-1992"></canvas>
        </section>

        <section>
            <h1>2007</h1>
            <canvas id="3d-2007"></canvas>
        </section>

        <section>
            <h1>2021</h1>
            <canvas id="3d-2007"></canvas>
        </section>
    </div>

    <section class="bg-white h-[500vh] justify-start">
        Shop
    </section>

</main>