<script>
    import Navbar from "../navbar.svelte";

    import { onMount } from "svelte";

    onMount(() => {
        let gridOptions = {
            autoSizeStrategy: {
                type: "fitGridWidth",
                defaultMinWidth: 120,
            },
            // Row Data: The data to be displayed.
            rowData: [],
            // Column Definitions: Defines the columns to be displayed.
            columnDefs: [
                { field: "TTS System", sortable: false },
                { field: "TTSDS", sortable: true, sortingOrder: ["desc"] },
                { field: "General", sortable: true, sortingOrder: ["desc"] },
                { field: "Speaker", sortable: true, sortingOrder: ["desc"] },
                { field: "Prosody", sortable: true, sortingOrder: ["desc"] },
                {
                    field: "Intelligibility",
                    sortable: true,
                    sortingOrder: ["desc"],
                },
                {
                    field: "Environment",
                    sortable: true,
                    sortingOrder: ["desc"],
                },
            ],
        };

        // load the leaderboard data from a csv file
        fetch("src/lib/leaderboard.csv")
            .then((response) => response.text())
            .then((data) => {
                // Parse the CSV data
                const rows = data.split("\n");
                const header = rows[0].split(",");
                const rowData = rows.slice(1).map((row) => {
                    const values = row.split(",");
                    return header.reduce((obj, key, i) => {
                        obj[key] = values[i];
                        return obj;
                    }, {});
                });

                // Set the row data
                gridOptions.rowData = rowData;

                console.log(gridOptions);

                const myGridElement = document.querySelector("#leaderboard");
                let gridApi = agGrid.createGrid(myGridElement, gridOptions);

                // sort by TTSDS score
                gridApi.applyColumnState({
                    state: [{ colId: "TTSDS", sort: "desc" }],
                    defaultState: { sort: null },
                });

                // set ag-header-cell-label to center
                const headerCells = document.querySelectorAll(
                    ".ag-header-cell-label",
                );
                headerCells.forEach((cell) => {
                    cell.style.justifyContent = "center";
                });
            });
    });

    function resizeIframe(obj) {
        obj.style.height =
            obj.contentWindow.document.documentElement.scrollHeight + "px";
    }
</script>

<Navbar />
<section class="section">
    <div class="container">
        <!-- <h1 class="title">Leaderboard</h1>
        <div
            id="leaderboard"
            style="height: 500px; width: 100%;"
            class="ag-theme-quartz"
        ></div> -->
        <iframe
            src="https://ttsds-benchmark.hf.space"
            frameborder="0"
            onload="resizeIframe(this)"
        ></iframe>
    </div>
</section>

<style>
    .logo {
        width: 500px;
    }
    .container {
        text-align: center;
    }
    iframe {
        display: block; /* iframes are inline by default */
        height: 100vh; /* Set height to 100% of the viewport height */
        width: 100%; /* Set width to 100% of the viewport width */
        border: none; /* Remove default border */
        overflow: show; /* Allow scrollbars if they are needed */
    }
</style>
