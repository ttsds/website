<script>
    import { onMount } from "svelte";

    // Default to English initially
    let selectedLanguage = "en";
    let isLoading = true;
    let loadError = null;

    // Language codes and their display names
    const languages = [
        { code: "en", name: "English" },
        { code: "zh", name: "中文 (Chinese)" },
        { code: "de", name: "Deutsch (German)" },
        { code: "es", name: "Español (Spanish)" },
        { code: "it", name: "Italiano (Italian)" },
        { code: "ja", name: "日本語 (Japanese)" },
        { code: "pl", name: "Polski (Polish)" },
        { code: "pt", name: "Português (Portuguese)" },
        { code: "tr", name: "Türkçe (Turkish)" },
        { code: "fr", name: "Français (French)" },
        { code: "ko", name: "한국어 (Korean)" },
        { code: "ar", name: "العربية (Arabic)" },
        { code: "ru", name: "Русский (Russian)" },
        { code: "nl", name: "Nederlands (Dutch)" },
    ];

    // Column names and their display names
    const columns = [
        { category: "Overall", label: "TTSDS2 Score" },
        { category: "Speaker", label: "Speaker" },
        { category: "Prosody", label: "Prosody" },
        { category: "Intelligibility", label: "Intelligibility" },
        { category: "Generic", label: "Generic" },
        { category: "Failure Rate", label: "Failure Rate (%)" },
    ];

    // Storage for all leaderboard data
    let leaderboardData = {};
    let consolidatedData = {};

    // Parse the CSV data
    async function loadLeaderboardData() {
        isLoading = true;
        loadError = null;

        try {
            const response = await fetch("/lib/leaderboard.csv");

            if (!response.ok) {
                throw new Error(
                    `Failed to load CSV: ${response.status} ${response.statusText}`,
                );
            }

            const csvText = await response.text();

            // Parse CSV data
            const rows = csvText.split("\n");

            if (rows.length < 2) {
                throw new Error("CSV file is empty or malformed");
            }

            const headers = rows[0].split(",");

            // Organize data by language and category
            languages.forEach((lang) => {
                leaderboardData[lang.code] = {};
                consolidatedData[lang.code] = {};
            });

            // Process each row
            for (let i = 1; i < rows.length; i++) {
                if (!rows[i].trim()) continue;

                const columns = rows[i].split(",");
                const system = columns[0];
                const url = columns[1];
                const language = columns[2];
                const category = columns[3];
                const score = parseFloat(columns[4]);

                // Skip if we don't have a valid language or score
                if (!language || isNaN(score)) continue;

                // Initialize category array if needed
                if (!leaderboardData[language][category]) {
                    leaderboardData[language][category] = [];
                }

                // Add this system to the category
                leaderboardData[language][category].push({
                    system,
                    url,
                    score,
                });

                // For consolidated view - collect data by system
                if (!consolidatedData[language][system]) {
                    consolidatedData[language][system] = {
                        url: url,
                        scores: {},
                    };
                }

                consolidatedData[language][system].scores[category] = score;
            }

            // Sort each category by score in descending order
            Object.keys(leaderboardData).forEach((lang) => {
                Object.keys(leaderboardData[lang]).forEach((category) => {
                    leaderboardData[lang][category].sort(
                        (a, b) => b.score - a.score,
                    );
                });
            });
        } catch (error) {
            console.error("Error loading leaderboard data:", error);
            loadError = error.message;
        } finally {
            isLoading = false;
        }
    }

    // Handle language selection
    function selectLanguage(lang) {
        selectedLanguage = lang;
    }

    // Function to determine if a score is the best (excluding ground truth)
    function isBestScore(systems, category, score) {
        if (!systems || !category) return false;

        // Find the best score that's not ground truth
        const nonGroundTruthScores = Object.entries(systems)
            .filter(([system]) => system !== "Ground Truth")
            .map(([_, data]) => data.scores[category])
            .filter((s) => s !== undefined);

        if (nonGroundTruthScores.length === 0) return false;

        const bestScore = Math.max(...nonGroundTruthScores);
        return score === bestScore && score !== undefined;
    }

    // Function to determine if a failure rate is the highest
    function isHighestFailureRate(systems, rate) {
        if (!systems || rate === undefined) return false;

        // Find all failure rates
        const failureRates = Object.entries(systems)
            .filter(([system]) => system !== "Ground Truth")
            .map(([_, data]) => data.scores["Failure Rate"])
            .filter((r) => r !== undefined);

        if (failureRates.length === 0) return false;

        const highestRate = Math.max(...failureRates);
        return rate === highestRate && rate > 0;
    }

    // Function to sort systems by Overall score
    function getSortedSystems(language) {
        if (!consolidatedData[language]) return [];

        return Object.entries(consolidatedData[language])
            .map(([system, data]) => ({
                system,
                ...data,
            }))
            .sort((a, b) => {
                // Sort by Overall score, then put Ground Truth at the top
                if (a.system === "Ground Truth") return -1;
                if (b.system === "Ground Truth") return 1;
                return (b.scores.Overall || 0) - (a.scores.Overall || 0);
            });
    }

    onMount(loadLeaderboardData);
</script>

<section class="container is-widescreen">
    <!-- Language selection buttons -->
    <div class="buttons is-centered">
        {#each languages as language}
            <button
                class="button {selectedLanguage === language.code
                    ? 'is-info'
                    : ''}"
                on:click={() => selectLanguage(language.code)}
                disabled={isLoading}
            >
                {language.name}
            </button>
        {/each}
    </div>

    <!-- Loading indicator -->
    {#if isLoading}
        <div class="has-text-centered">
            <p class="has-text-light">Loading leaderboard data...</p>
            <progress class="progress is-info" max="100"></progress>
        </div>
        <!-- Error message -->
    {:else if loadError}
        <div class="notification is-danger">
            <p>Error loading leaderboard data: {loadError}</p>
            <button class="button is-info mt-3" on:click={loadLeaderboardData}
                >Try Again</button
            >
        </div>
        <!-- No data for selected language -->
    {:else if !consolidatedData[selectedLanguage] || Object.keys(consolidatedData[selectedLanguage]).length === 0}
        <div class="notification is-warning">
            <p>
                No data available for {languages.find(
                    (l) => l.code === selectedLanguage,
                )?.name || selectedLanguage}
            </p>
        </div>
        <!-- Consolidated leaderboard table -->
    {:else}
        <div class="table-wrapper">
            <div class="table-container">
                <table class="table">
                    <thead>
                        <tr>
                            <th>Rank</th>
                            <th>System</th>
                            {#each columns as column}
                                {#if column.category === "Failure Rate"}
                                    <th
                                        ><abbr
                                            title="Percentage of utterances with incorrect lexical content"
                                            >{column.label}</abbr
                                        ></th
                                    >
                                {:else}
                                    <th>{column.label}</th>
                                {/if}
                            {/each}
                        </tr>
                    </thead>
                    <tbody>
                        {#each getSortedSystems(selectedLanguage) as system, i}
                            <tr
                                class={system.system === "Ground Truth"
                                    ? "ground-truth"
                                    : ""}
                            >
                                <td>{i + 1}</td>
                                <td>
                                    {#if system.url && system.url !== ""}
                                        <a
                                            href={system.url}
                                            target="_blank"
                                            rel="noopener noreferrer"
                                        >
                                            {#if system.system === "Ground Truth"}
                                                <em>{system.system}</em>
                                            {:else}
                                                {system.system}
                                            {/if}
                                        </a>
                                    {:else if system.system === "Ground Truth"}
                                        <em>{system.system}</em>
                                    {:else}
                                        {system.system}
                                    {/if}
                                </td>
                                {#each columns as column}
                                    {#if column.category === "Failure Rate"}
                                        {#if system.system === "Ground Truth"}
                                            <td>—</td>
                                        {:else}
                                            <td
                                                class:has-text-danger={isHighestFailureRate(
                                                    consolidatedData[
                                                        selectedLanguage
                                                    ],
                                                    system.scores[
                                                        column.category
                                                    ],
                                                )}
                                            >
                                                {system.scores[
                                                    column.category
                                                ] !== undefined
                                                    ? system.scores[
                                                          column.category
                                                      ].toFixed(1)
                                                    : "—"}
                                            </td>
                                        {/if}
                                    {:else}
                                        <td
                                            class:has-text-success={isBestScore(
                                                consolidatedData[
                                                    selectedLanguage
                                                ],
                                                column.category,
                                                system.scores[column.category],
                                            ) &&
                                                system.system !==
                                                    "Ground Truth"}
                                        >
                                            {system.scores[column.category] !==
                                            undefined
                                                ? system.scores[
                                                      column.category
                                                  ].toFixed(2)
                                                : "—"}
                                        </td>
                                    {/if}
                                {/each}
                            </tr>
                        {/each}
                    </tbody>
                </table>
            </div>
        </div>
    {/if}
</section>

<style>
    .table-wrapper {
        margin: 2rem 0;
    }

    .table {
        text-align: center;
        margin: 0 auto;
        background-color: rgba(255, 255, 255, 0.1);
        color: white;
    }

    .table th {
        text-align: center !important;
        color: white;
    }

    .table-container {
        max-height: 70vh;
        overflow-y: auto;
        border-radius: 4px;
    }

    .has-text-success {
        color: #48c774 !important;
        font-weight: bold;
    }

    .has-text-danger {
        color: #f14668 !important;
        font-weight: bold;
    }

    .buttons {
        flex-wrap: wrap;
        justify-content: center;
    }

    a {
        color: #3e8ed0;
    }

    a:hover {
        color: #48c774;
    }

    .notification {
        margin: 2rem auto;
        max-width: 600px;
        text-align: center;
    }

    .progress {
        max-width: 500px;
        margin: 2rem auto;
    }

    .mt-3 {
        margin-top: 1rem;
    }

    .ground-truth {
        background-color: rgba(255, 255, 255, 0.05);
    }

    em {
        font-style: italic;
    }

    abbr {
        cursor: help;
        text-decoration: underline dotted;
    }
</style>
