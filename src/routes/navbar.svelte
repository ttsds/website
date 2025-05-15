<script>
    import { onMount } from "svelte";

    onMount(() => {
        // Get all "navbar-burger" elements
        const $navbarBurgers = Array.prototype.slice.call(
            document.querySelectorAll(".navbar-burger"),
            0,
        );

        // Add a click event on each of them
        $navbarBurgers.forEach((el) => {
            el.addEventListener("click", () => {
                // Get the target from the "data-target" attribute
                let target = document.getElementById(el.dataset.target);
                // Toggle the "is-active" class on both the "navbar-burger" and the "navbar-menu"
                el.classList.toggle("is-active");
                target.classList.toggle("is-active");
            });
        });

        // check current page and highlight the corresponding navbar item
        let path = "" + window.location;
        const navbarItems = document.querySelectorAll(".navbar-item");
        navbarItems.forEach((item) => {
            if (item.href === path) {
                // check if no-active class is present
                if (!item.classList.contains("no-active")) {
                    item.classList.add("is-active");
                }
            }
        });

        window.addEventListener("hashchange", () => {
            let path = "" + window.location;
            const navbarItems = document.querySelectorAll(".navbar-item");
            navbarItems.forEach((item) => {
                if (item.href === path) {
                    if (!item.classList.contains("no-active")) {
                        item.classList.add("is-active");
                    }
                } else {
                    item.classList.remove("is-active");
                }
            });
        });

        const navbarLinks = document.querySelectorAll(".navbar-link");
        navbarLinks.forEach((item) => {
            // remove # from path
            path = path.split("#")[0];
            if (item.href === path) {
                item.classList.add("is-active");
            }
        });
    });
</script>

<nav class="navbar" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
        <a class="navbar-item no-active" href="/">
            <img src="logo_dark_crop.png" alt="TTSDS Benchmark" />
            &nbsp;
            <strong>TTSDS</strong>
        </a>

        <a
            role="button"
            class="navbar-burger"
            aria-label="menu"
            aria-expanded="false"
            data-target="navbarBasicExample"
        >
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
        </a>
    </div>

    <div id="navbarBasicExample" class="navbar-menu">
        <div class="navbar-end">
            <a
                class="navbar-item"
                href="https://arxiv.org/abs/2407.12707"
                target="_blank"
            >
                Paper
            </a>
            <a
                class="navbar-item"
                href="https://github.com/ttsds/ttsds"
                target="_blank"
            >
                Code
            </a>
            <a class="navbar-item" href="/leaderboard"> Leaderboard </a>

            <div class="navbar-item has-dropdown is-hoverable">
                <a class="navbar-link" href="/factors"> Factors </a>

                <div class="navbar-dropdown" data-href="/factors">
                    <a class="navbar-item" href="/factors#environment">
                        Environment
                    </a>
                    <a class="navbar-item" href="/factors#speaker"> Speaker </a>
                    <a class="navbar-item" href="/factors#prosody"> Prosody </a>
                    <a class="navbar-item" href="/factors#intelligibility">
                        Intelligibility
                    </a>
                    <a class="navbar-item" href="/factors#general"> General </a>
                </div>
            </div>
            <div class="navbar-item">
                <div class="buttons">
                    <a
                        href="https://huggingface.co/spaces/ttsds/benchmark"
                        class="button is-danger"
                        target="_blank"
                    >
                        <strong>Submit</strong>
                    </a>
                </div>
            </div>
            <div class="navbar-item has-dropdown is-hoverable">
                <a class="navbar-link" href="/contribute"> Contribute </a>

                <div class="navbar-dropdown">
                    <a
                        class="navbar-item"
                        href="https://huggingface.co/spaces/ttsds/benchmark/discussions/1"
                        target="_blank"
                    >
                        Suggest a Benchmark
                    </a>
                    <a
                        class="navbar-item"
                        href="https://huggingface.co/spaces/ttsds/benchmark/discussions/2"
                        target="_blank"
                    >
                        Suggest a Model
                    </a>
                    <a
                        class="navbar-item"
                        href="https://huggingface.co/spaces/ttsds/benchmark/discussions/3"
                        target="_blank"
                    >
                        Suggest a Tag
                    </a>
                </div>
            </div>
            <a class="navbar-item" href="/#about"> About </a>
        </div>
    </div>
</nav>

<style>
    .navbar-link:not(.is-arrowless)::after {
        border-color: var(--text-color);
    }
</style>
