<script>
    const ROW_HEIGHT = 40; // Approximate height of each row in pixels

    const rows = [
        {
            metric: "TTSDSv2",
            clean: [0.75, 0.69, 0.73],
            noisy: [0.59, 0.54, 0.71],
            wild: [0.75, 0.71, 0.75],
            kids: [0.61, 0.5, 0.7],
            overall: [0.67, 0.08],
        },
        {
            metric: "TTSDSv1",
            clean: [0.6, 0.62, 0.52],
            noisy: [0.49, 0.61, 0.66],
            wild: [0.67, 0.57, 0.67],
            kids: [0.7, 0.52, 0.6],
            overall: [0.6, 0.06],
        },
        {
            metric: "X‑Vector",
            clean: [0.46, 0.42, 0.56],
            noisy: [0.4, 0.29, 0.77],
            wild: [0.82, 0.82, 0.62],
            kids: [0.7, 0.57, 0.75],
            overall: [0.6, 0.17],
        },
        {
            metric: "RawNet3",
            clean: [0.36, 0.26, 0.52],
            noisy: [0.44, 0.37, 0.82],
            wild: [0.85, 0.8, 0.64],
            kids: [0.73, 0.61, 0.77],
            overall: [0.6, 0.2],
        },
        {
            metric: "SQUIM",
            clean: [0.68, 0.46, 0.37],
            noisy: [0.48, 0.48, 0.6],
            wild: [0.62, 0.75, 0.79],
            kids: [0.57, 0.55, 0.45],
            overall: [0.57, 0.12],
        },
        {
            metric: "ECAPA‑TDNN",
            clean: [0.36, 0.29, 0.47],
            noisy: [0.29, 0.22, 0.72],
            wild: [0.81, 0.78, 0.58],
            kids: [0.69, 0.6, 0.72],
            overall: [0.54, 0.2],
        },
        {
            metric: "DNSMOS",
            clean: [0.41, 0.37, 0.22],
            noisy: [0.57, 0.36, 0.22],
            wild: [0.35, 0.28, 0.03],
            kids: [0.31, 0.1, 0.28],
            overall: [0.29, 0.14],
        },
        {
            metric: "Content Enjoyment",
            clean: [0.6, 0.46, 0.32],
            noisy: [0.58, 0.53, 0.21],
            wild: [0.19, 0.1, 0.11],
            kids: [-0.02, -0.12, -0.1],
            overall: [0.24, 0.25],
        },
        {
            metric: "Content Usefulness",
            clean: [0.49, 0.37, 0.3],
            noisy: [0.6, 0.58, 0.13],
            wild: [0.35, 0.24, 0.22],
            kids: [-0.09, -0.21, -0.13],
            overall: [0.24, 0.26],
        },
        {
            metric: "Production Quality",
            clean: [0.49, 0.33, 0.21],
            noisy: [0.55, 0.48, 0.04],
            wild: [0.21, 0.16, 0.12],
            kids: [0.03, -0.08, -0.05],
            overall: [0.21, 0.21],
        },
        {
            metric: "UTMOSv2",
            clean: [0.39, 0.25, 0.09],
            noisy: [0.34, 0.36, 0.19],
            wild: [0.16, 0.14, -0.04],
            kids: [0.05, 0.03, -0.02],
            overall: [0.16, 0.14],
        },
        {
            metric: "FAD (CLAP)",
            clean: [-0.22, 0.06, -0.01],
            noisy: [0.45, 0.3, 0.16],
            wild: [-0.03, 0.08, 0.25],
            kids: [0.12, 0.26, 0.04],
            overall: [0.12, 0.17],
        },
        {
            metric: "UTMOS",
            clean: [0.51, 0.3, 0.31],
            noisy: [0.47, 0.29, 0.0],
            wild: [-0.12, -0.12, -0.26],
            kids: [-0.02, -0.18, -0.04],
            overall: [0.1, 0.25],
        },
        {
            metric: "STOI",
            clean: [-0.11, 0.01, 0.02],
            noisy: [-0.06, 0.0, 0.19],
            wild: [0.07, 0.41, 0.24],
            kids: [-0.32, -0.08, 0.05],
            overall: [0.03, 0.18],
        },
        {
            metric: "PESQ",
            clean: [0.01, -0.16, 0.27],
            noisy: [-0.34, 0.0, 0.07],
            wild: [-0.14, 0.01, -0.06],
            kids: [-0.08, -0.04, -0.38],
            overall: [-0.07, 0.17],
        },
        {
            metric: "NISQA",
            clean: [0.05, 0.0, 0.06],
            noisy: [0.05, -0.21, -0.53],
            wild: [-0.32, -0.33, -0.64],
            kids: [-0.29, -0.27, -0.46],
            overall: [-0.24, 0.23],
        },
        {
            metric: "MCD",
            clean: [-0.46, -0.37, -0.27],
            noisy: [-0.45, -0.58, -0.74],
            wild: [-0.33, -0.45, -0.51],
            kids: [-0.31, -0.13, -0.38],
            overall: [-0.42, 0.15],
        },
    ];
</script>

<div class="table-wrapper">
    <div class="table-container">
        <table class="table">
            <thead>
                <tr>
                    <th rowspan="2">Metric</th>
                    <th colspan="3"
                        ><abbr title="LibriTTS test-clean">Clean</abbr></th
                    >
                    <th colspan="3"
                        ><abbr title="Scraped LibriVox">Noisy</abbr></th
                    >
                    <th colspan="3"
                        ><abbr title="Scraped YouTube">Wild</abbr></th
                    >
                    <th colspan="3"><abbr title="MyST">Kids</abbr></th>
                    <th colspan="2">Overall</th>
                </tr>
                <tr>
                    <th>MOS</th><th>CMOS</th><th>SMOS</th>
                    <th>MOS</th><th>CMOS</th><th>SMOS</th>
                    <th>MOS</th><th>CMOS</th><th>SMOS</th>
                    <th>MOS</th><th>CMOS</th><th>SMOS</th>
                    <th>&mu;</th><th>&sigma;</th>
                </tr>
            </thead>
            <tbody>
                {#each rows as row}
                    <tr>
                        <td
                            class:has-text-danger={row.metric === "TTSDSv1" ||
                                row.metric === "TTSDSv2"}
                            class:has-text-weight-bold={row.metric ===
                                "TTSDSv1" || row.metric === "TTSDSv2"}
                            >{row.metric}</td
                        >
                        {#each row.clean as value}
                            <td class:has-background-link={value >= 0.5}
                                >{value}</td
                            >
                        {/each}
                        {#each row.noisy as value}
                            <td class:has-background-link={value >= 0.5}
                                >{value}</td
                            >
                        {/each}
                        {#each row.wild as value}
                            <td class:has-background-link={value >= 0.5}
                                >{value}</td
                            >
                        {/each}
                        {#each row.kids as value}
                            <td class:has-background-link={value >= 0.5}
                                >{value}</td
                            >
                        {/each}
                        {#each row.overall as value}
                            <td class:has-background-link={value >= 0.5}
                                >{value}</td
                            >
                        {/each}
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>
</div>

<style>
    .table-wrapper {
        position: relative;
        margin: 2rem 0;
    }

    .table {
        /* center the text in the table */
        text-align: center;
        font-size: 0.8em;
        border-radius: 4px;
    }

    .table th {
        text-align: center !important;
    }

    .table-container {
        max-height: 80vh;
        overflow-y: auto;
        border-radius: 4px;
    }

    :global(.has-background-link) {
        color: white;
    }
    .table-wrapper {
        margin: 0 auto;
    }
</style>
