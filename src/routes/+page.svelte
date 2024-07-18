<script>
    import Navbar from "./navbar.svelte";
    import Card from "./card.svelte";
</script>

<Navbar />
<section class="section">
    <div class="container">
        <img src="logo-dark.png" class="logo" />
        <h1 class="title">Text to Speech Distribution Score</h1>
        <p class="subtitle">
            Benchmarking the performance of TTS systems across various factors.
        </p>

        <p>
            As many recent Text-to-Speech (TTS) models have shown, synthetic
            audio can be close to real human speech. However, traditional
            evaluation methods for TTS systems need an update to keep pace with
            these new developments. Our TTSDS benchmark assesses the quality of
            synthetic speech by considering factors like prosody, speaker
            identity, and intelligibility. By comparing these factors with both
            real speech and noise datasets, we can better understand how close
            synthetic speech is to human speech.
        </p>

        <Card
            title="More Information"
            content="
            <p>
                More details can be found in our paper <a
                    href='https://arxiv.org/abs/2407.12707'
                    ><em>TTSDS -- Text-to-Speech Distribution Score</em></a
                >.
            </p>

            <h2>Reproducibility</h2>
            <p>
                To reproduce our results, check out our repository <a
                    href='https://github.com/ttsds/ttsds'>here</a
                >.
            </p>

            <h2>Credits</h2>
            <p>
                This benchmark is inspired by <a
                    href='https://huggingface.co/spaces/TTS-AGI/TTS-Arena'
                    >TTS Arena</a
                > which instead focuses on the subjective evaluation of TTS models. Our
                benchmark would not be possible without the many open-source TTS models
                on Hugging Face and GitHub. Additionally, our benchmark uses the following
                datasets:
            </p>
            <ul>
                <li>
                    <a href='https://keithito.com/LJ-Speech-Dataset/'>LJSpeech</a>
                </li>
                <li><a href='https://www.openslr.org/60/'>LibriTTS</a></li>
                <li>
                    <a href='https://datashare.ed.ac.uk/handle/10283/2950'>VCTK</a>
                </li>
                <li><a href='https://commonvoice.mozilla.org/'>Common Voice</a></li>
                <li><a href='https://github.com/karolpiczak/ESC-50'>ESC-50</a></li>
            </ul>
            <p>And the following metrics/representations/tools:</p>
            <ul>
                <li><a href='https://arxiv.org/abs/2006.11477'>Wav2Vec2</a></li>
                <li><a href='https://arxiv.org/abs/2006.11477'>Hubert</a></li>
                <li><a href='https://arxiv.org/abs/2110.13900'>WavLM</a></li>
                <li>
                    <a
                        href='https://en.wikipedia.org/wiki/Perceptual_Evaluation_of_Speech_Quality'
                        >PESQ</a
                    >
                </li>
                <li><a href='https://arxiv.org/abs/2204.05841'>VoiceFixer</a></li>
                <li>
                    <a href='https://www.cs.cmu.edu/~robust/Papers/KimSternIS08.pdf'
                        >WADA SNR</a
                    >
                </li>
                <li><a href='https://arxiv.org/abs/2212.04356'>Whisper</a></li>
                <li>
                    <a href='https://huggingface.co/cdminix/masked_prosody_model'
                        >Masked Prosody Model</a
                    >
                </li>
                <li>
                    <a
                        href='https://github.com/JeremyCCHsu/Python-Wrapper-for-World-Vocoder'
                        >PyWorld</a
                    >
                </li>
                <li><a href='https://arxiv.org/abs/2210.17016'>WeSpeaker</a></li>
                <li><a href='https://github.com/yistLin/dvector'>D-Vector</a></li>
            </ul>"
        />

        <p>
            Authors: Christoph Minixhofer, Ondřej Klejch, and Peter Bell
            <br />
            University of Edinburgh.
            <br />
            Contact (Christoph Minixhofer): firstname.lastname@ed.ac.uk
        </p>

        <img src="edi-logo.png" class="edi" width="500" />
    </div>
</section>

<style>
    .logo {
        width: 500px;
        mix-blend-mode: lighten;
    }
    .edi {
        background-color: white;
    }
    .container {
        text-align: center;
    }
    li {
        list-style-type: none !important;
    }
</style>
