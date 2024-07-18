<script>
    import Navbar from "../navbar.svelte";
    import Card from "../card.svelte";
</script>

<Navbar />
<section class="section">
    <div class="container">
        <h1 class="title">TTSDS Factors</h1>
        <p class="subtitle">
            The factors used to evaluate the performance of TTS systems.
        </p>
    </div>
    <div class="grid">
        <Card
            title="General"
            content="<p>
                        Measures the distribution of the speech without any
                        assumptions. We use self-supervised learning (SSL)
                        representations of the speech.
                    </p>
                    <ul>
                        <li><a href='https://arxiv.org/abs/2106.07447'>Hubert</a></li>
                        <li>
                            <a href='https://arxiv.org/abs/2006.11477'>wav2vec 2.0</a>
                        </li>
                        <li><a href='https://arxiv.org/abs/2110.13900'>WavLM</a></li>
                    </ul>"
        />
        <Card
            title="Environment"
            content="<p>
                        Measures noise or distortion present in the speech. We
                        use correlates for signal-to-noise ratio (SNR).
                    </p>
                    <ul>
                        <li>
                            <a
                                href='https://en.wikipedia.org/wiki/Perceptual_Evaluation_of_Speech_Quality'
                                >PESQ</a
                            >
                            +
                            <a href='https://arxiv.org/abs/2204.05841'
                                >VoiceFixer</a
                            >
                        </li>
                        <li>
                            <a
                                href='https://www.cs.cmu.edu/~robust/Papers/KimSternIS08.pdf'
                                >WADA SNR</a
                            >
                        </li>
                    </ul>"
        />
        <Card
            title="Intelligibility"
            content="<p>
                        Measures how easy the content of the speech is to
                        recognize. We use WER obtained using the transcripts
                        provided to the TTS systems and ASR systems.
                    </p>
                    <ul>
                        <li>
                            <a href='https://arxiv.org/abs/2006.11477'>wav2vec 2.0</a>
                        </li>
                        <li><a href='https://arxiv.org/abs/2212.04356'>Whisper</a></li>
                    </ul>"
        />
        <Card
            title="Prosody"
            content="<p>
                        Measures how realistic the prosody of the speech is. We use
                        a pitch extractor, a SSL representation of the prosody and a
                        proxy for duration and speaking rate.
                    </p>
                    <ul>
                        <li>
                            <a href='https://arxiv.org/abs/2106.07447'>Hubert</a> + Token
                            Durations
                        </li>
                        <li>
                            <a
                                href='https://huggingface.co/cdminix/masked_prosody_model'
                                >Masked Prosody Model</a
                            >
                        </li>
                        <li>
                            <a
                                href='https://github.com/JeremyCCHsu/Python-Wrapper-for-World-Vocoder/tree/master'
                                >PyWorld</a
                            > Pitch
                        </li>
                    </ul>"
        />
        <Card
            title="Speaker"
            content="<p>
                        Measures how close the speakers are to real ones. We use
                        representations obtained from speaker verification systems.
                    </p>
                    <ul>
                        <li>
                            <a href='https://arxiv.org/abs/2210.17016'>WeSpeaker</a>
                        </li>
                        <li>
                            <a href='https://github.com/yistLin/dvector'>D-Vector</a
                            >
                        </li>
                    </ul>"
        />
    </div>
</section>

<style>
    .logo {
        width: 500px;
    }
    .container {
        text-align: center;
    }
    h1 {
        margin-top: 2rem;
    }
</style>
