# LanguageLeapAI

![](docs/screenshots/LanguageLeapAI_logo.png?raw=true)

LEAP across Language barriers by using AI to converse with other online users from across the globe!
**LanguageLeapAI** aims to provide you a real-time language AI assistant that can understand and speak your desired language fluently. 
*(Targeted towards English to Japanese as of right now)*


## Integration of AI Entities

This project integrates 3 free and open-source AI systems:

1. [WhisperAI](https://github.com/openai/whisper): General-purpose Speech Recognition Model developed by OpenAI that can perform multilingual speech
recognition.
2. [DeepL Translator](https://www.deepl.com/translator): Powered by neural networks and the latest AI innovations for natural-sounding translations
3. [Voicevox](https://voicevox.hiroshiba.jp/): Japanese Deep-Learning AI Voice Synthesizer


WhisperAI and Voicevox both have docker images available on DockerHub, so we will be building and running them both via a [Docker Compose file](docker-compose.yml).
DeepL can be interacted with by signing up for a free plan and interacting with its [REST API](https://www.deepl.com/pro-api?cta=header-pro-api/) up to 500,000 character limit / month

![](docs/screenshots/ai_integrations.png?raw=true)

## How it works

**LanguageLeapAI** is made up of 2 main python programs.


### Voice Translator

The first, [voice_translator.py](src/voice_translator.py), records your microphone whenever a push-to-talk key is held down on the keyboard.
Once this key is released, it saves your voice in an audio file which is then sent to WhisperAI's transcribe endpoint which runs Automatic Speech Recognition (ASR) on it.
After a response containing your speech as text is received, this text is then translated using DeepL's REST API. 

The translated text is then sent to Voicevox which performs text-to-speech and generates an audio file voiced in Japanese.
This file is then played to your target application's microphone input and your speakers/headphones.

Since Voicevox only takes in Japanese text as input and generates speech in Japanese, the project is technically only limited to Japanese as the target language.
However, Voicevox can be replaced with any other text to speech program that can speak your desired language for limitless possibilities.


### Audio Subtitler

The second, [subtitler.py](src/subtitler.py), records your application's audio output and listens in the background for any speech.
Once it has detected that a phrase/sentence is complete, it saves the audio into a wav file and sends it to WhisperAI's translate endpoint which translates the speech from the target language to English.

This English text is then displayed on screen using python's tkinter module, essentially acting as subtitles.


## Applications

**LanguageLeapAI's** target audience is for users who want to chat with another but do not speak the same language.
An example is an English-speaking user playing an online game in the Japan server but wants to use voice chat despite not knowing Japanese.

By running both [subtitler.py](src/subtitler.py) and [voice_translator.py](src/voice_translator.py), they can understand their fellow Japanese teammates by reading the english subtitles generated in real time.
They can also speak English and the Japanese teammates will instead hear the translated Japanese speech generated by Voicevox.

However,  this is not the only application of **LanguageLeapAI**.

### Only using Audio Subtitler

User simply wants to understand what is being said with no need to speak. E.g. Watching a video / stream / movie in another language without subtitles.
The user can choose to not run [voice_translator.py](src/voice_translator.py) and simply use [subtitler.py](src/subtitler.py).

### Only using Voice Translator

User understands the language enough to listen and understand, but is afraid to speak the language for various reasons, e.g. Anonymity / Fear of messing up or offending.
The user can choose to not run [subtitler.py](src/subtitler.py) and simply use [voice_translator.py](src/voice_translator.py).


## Setup

Setting up **LanguageLeapAI** requires 3 crucial steps, so don't miss out on any of them!
1. [Installing Services and Dependencies](docs/INSTALLATION.md)
2. [Audio Routing](#audio-routing)
3. [Writing your Environment file](docs/ENV.md)


## Usage

To run **LanguageLeapAI**, you need to first run the WhisperAI and Voicevox docker containers. Then you can run the 2 python scripts.

### Docker

Run these commands in the root folder containing the [docker-compose.yml](docker-compose.yml) file.

To run both WhisperAI and Voicevox:

```docker-compose up -d```

For users who only want to run the audio subtitler, they have the option of only running WhisperAI's docker container:

```docker-compose start whisper```

For users who only want to run Voicevox:

```docker-compose start voicevox```

To stop running the containers:

```docker-compose down```


### Python Program

Run these commands in the [src/](src) folder.

To run the Audio Subtitler:

```python subtitler.py```

To run the Voice Translator:

```python voice_translate.py```

To stop the python scripts, simply press `Ctrl+C` in the terminal.

## License

The code of LanguageLeapAI is released under the MIT License. See [LICENSE](LICENSE) for further details.
