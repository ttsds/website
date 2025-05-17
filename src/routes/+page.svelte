<script>
    import PerformanceTable from "./PerformanceTable.svelte";
    import Leaderboard from "./Leaderboard.svelte";

    // Calculate days until benchmark update (July 5th, 2025)
    const updateDate = new Date("2025-07-05");
    const today = new Date();
    const daysRemaining = Math.ceil(
        (updateDate - today) / (1000 * 60 * 60 * 24),
    );

    // Format date
    const formattedDate = updateDate.toLocaleDateString("en-US", {
        year: "numeric",
        month: "long",
        day: "numeric",
    });
</script>

<!-- <Navbar /> -->
<section class="section">
    <div class="container main">
        <section class="container is-max-desktop">
            <img src="logo_dark_crop.png" class="logo" />
            <h1 class="title">TTSDS2: Text to Speech Distribution Score</h1>

            <p>
                TTSDS2 is a collection of resources for evaluating the
                performance of text-to-speech (TTS) models.
            </p>
            <p>
                It <a href="#correlation">strongly correlates</a> with human judgement
                of speech quality for recent TTS models.
            </p>
            <br />
            The original TTSDS paper was
            <a href="https://ieeexplore.ieee.org/abstract/document/10832178"
                >presented at the IEEE SLT 2024 conference</a
            >.
            <br />
            TTSDS2, the multi-language and multi-dataset extension of TTSDS, is currently
            under review.
            <!-- available on
            <a href="TBD">arXiv</a>. -->
            <br />
            <br />
        </section>

        <section class="container is-max-desktop">
            <div class="columns">
                <div class="column">
                    <a href="#leaderboard" class="button is-large is-danger"
                        >Leaderboard</a
                    >
                    <p>
                        A leaderboard of state-of-the-art TTS models and their
                        TTSDS2 scores.
                    </p>
                </div>
                <div class="column">
                    <a
                        href="https://huggingface.co/ttsds"
                        class="button is-large">Datasets</a
                    >
                    <p>Datasets for TTS evaluation and MOS prediction.</p>
                </div>
                <div class="column">
                    <a
                        href="https://replicate.com/ttsds"
                        class="button is-large">Systems</a
                    >
                    <p>A collection of the evaluated TTS models.</p>
                </div>
            </div>
            <br />
            <br />
            <p>
                You can also <a
                    href="https://github.com/ttsds/ttsds"
                    class="button inline"
                    ><img src="github.png" class="github" />use TTSDS2 locally</a
                >
                or
                <a
                    href="https://huggingface.co/spaces/ttsds/README/discussions/1"
                    class="button inline"
                    ><img src="huggingface.png" class="github" />request a model
                    evaluation</a
                >
            </p>
            <br />
        </section>

        <section class="container is-widescreen" id="leaderboard">
            <section
                class="hero is-fullheight is-light has-background-link-dark has-text-light"
            >
                <div class="hero-head">
                    <p class="title is-2 has-text-light is-centered">
                        TTSDS2 Leaderboard
                    </p>
                    <p class="subtitle has-text-light is-centered">Q4 2024</p>
                    <a
                        href="https://huggingface.co/datasets/ttsds/leaderboard_data"
                        class="button inline"
                        ><img
                            src="huggingface.png"
                            class="github"
                        />ttsds/leaderboard_data</a
                    >
                    <Leaderboard />
                </div>
            </section>
        </section>

        <section class="container is-max-desktop">
            <br />
            <nav class="level">
                <div class="level-item has-text-centered">
                    <div>
                        <p class="heading">Replicate Runs</p>
                        <p class="title">12,654</p>
                    </div>
                </div>
                <div class="level-item has-text-centered">
                    <div>
                        <p class="heading">TTS Systems Evaluated</p>
                        <p class="title">45</p>
                    </div>
                </div>
                <div class="level-item has-text-centered">
                    <div>
                        <p class="heading">Languages</p>
                        <p class="title">14</p>
                    </div>
                </div>
                <div class="level-item has-text-centered">
                    <div>
                        <p class="heading">Hearts & Stars</p>
                        <p class="title">78</p>
                    </div>
                </div>
                <div class="level-item has-text-centered">
                    <div>
                        <p class="heading">Next Benchmark Update</p>
                        <p class="title">{daysRemaining} days</p>
                    </div>
                </div>
            </nav>
            <br />
        </section>

        <section class="container is-widescreen" id="correlation">
            <section
                class="hero is-fullheight is-light has-background-link-dark has-text-light"
            >
                <div class="hero-head">
                    <p class="title is-2 has-text-light is-centered">
                        Correlations with MOS
                    </p>
                    <section class="container is-widescreen">
                        <div class="columns">
                            <div class="column">
                                <p>
                                    This table shows the correlation between the
                                    TTSDS2 score and the mean opinion score
                                    (MOS), comparative mean opinion score
                                    (CMOS), and speaker similarity mean opinion
                                    score (SMOS) for four datasets. <span
                                        class="has-text-weight-bold has-text-danger"
                                        >TTSDS2 is the only metric that
                                        correlates strongly with MOS for all
                                        datasets.</span
                                    >
                                </p>
                            </div>
                            <div class="column is-three-quarters">
                                <PerformanceTable />
                            </div>
                        </div>
                    </section>
                </div>
            </section>
        </section>

        <br />
        <p>
            Authors: Christoph Minixhofer, Ondřej Klejch, and Peter Bell @
            University of Edinburgh.
            <br />
            Contact (Christoph Minixhofer): {"christoph" +
                "." +
                "minixhofer" +
                "@ed.ac.uk"}
        </p>
        <br />

        <img src="edi-logo.png" class="edi" />
    </div>
</section>

<style>
    .logo {
        max-width: 14em;
        mix-blend-mode: lighten;
    }
    .edi {
        width: 15em;
    }
    .container.is-max-desktop {
        text-align: center;
        margin: 0 auto;
    }
    li {
        list-style-type: none !important;
    }
    .is-centered {
        text-align: center;
    }
    .container.is-widescreen {
        margin: 0 auto;
        position: relative;
        width: 100%;
    }
    .container .hero {
        width: 100vw;
        position: relative;
        left: calc(-50vw + 50%);
    }
    .hero .title {
        margin-top: 1em;
    }
    .hero p {
        position: relative;
        top: 50%;
        transform: translateY(-50%);
        margin-left: 1em;
    }
    @media (max-width: 768px) {
        .container .hero {
            padding: 0 3em;
        }
        .hero .title {
            margin-top: 0.5em;
        }
        .hero p {
            top: 0;
            transform: translateY(0);
        }
    }
    .button.inline {
        transform: translateY(-0.5em);
    }
    .github {
        width: 1em;
        margin-right: 0.5em;
    }
    .countdown {
        font-weight: bold;
        color: #f14668;
    }
</style>
