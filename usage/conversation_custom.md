# Podcastfy Conversation Configuration

Podcastfy offers a range of customization options to tailor your AI-generated podcasts. This document outlines how you can adjust parameters such as conversation style, word count, and dialogue structure to suit your specific needs. Whether you're creating educational content, storytelling experiences, or anything in between, these configuration options allow you to fine-tune your podcast's tone, length, and format.  See [Notes of Caution](#notes-of-caution) before starting to customize to avoid unexpected results.


## Table of Contents

1. [Parameters](#parameters)
2. [Customization Examples](#customization-examples)
   1. [Academic Debate](#academic-debate)
   2. [Storytelling Adventure](#storytelling-adventure)
3. [Customization Scenarios](#customization-scenarios)
   1. [Using the Python Package](#using-the-python-package)
   2. [Using the CLI](#using-the-cli)
   3. [Dev Config](#dev-config)
4. [Notes of Caution](#notes-of-caution)


## Parameters

| Parameter | Default Value | Type | Description |
|-----------|---------------|------|-------------|
| word_count | 2000 | int | Target word count for the generated content |
| conversation_style | ["engaging", "fast-paced", "enthusiastic"] | list[str] | Styles to apply to the conversation |
| roles_person1 | "main summarizer" | str | Role of the first speaker |
| roles_person2 | "questioner/clarifier" | str | Role of the second speaker |
| dialogue_structure | ["Introduction", "Main Content Summary", "Conclusion"] | list[str] | Structure of the dialogue |
| podcast_name | "PODCASTFY" | str | Name of the podcast |
| podcast_tagline | "YOUR PERSONAL GenAI PODCAST" | str | Tagline for the podcast |
| output_language | "English" | str | Language of the output |
| engagement_techniques | ["rhetorical questions", "anecdotes", "analogies", "humor"] | list[str] | Techniques to engage the audience |
| creativity | 0 | int | Level of creativity/temperature (0-1) |

Podcastfy uses the the default conversation configuration stored in [podcastfy/conversation_config.yaml](https://github.com/souzatharsis/podcastfy/blob/main/podcastfy/conversation_config.yaml).

## Customization Examples

These examples demonstrate how conversations can be altered to suit different purposes, from academic rigor to creative storytelling. The comments explain the rationale behind each choice, helping users understand how to tailor the configuration to their specific needs.

### Academic Debate

This configuration transforms the podcast into a formal academic debate, encouraging deep analysis and critical thinking. It's designed for educational content or in-depth discussions on complex topics.

```python
{
    "word_count": 3000,  # Longer to allow for detailed arguments
    "conversation_style": ["formal", "analytical", "critical"],  # Appropriate for academic discourse
    "roles_person1": "thesis presenter",  # Presents the main argument
    "roles_person2": "counterargument provider",  # Challenges the thesis
    "dialogue_structure": [
        "Opening Statements",
        "Thesis Presentation",
        "Counterarguments",
        "Rebuttals",
        "Closing Remarks"
    ],  # Mimics a structured debate format
    "podcast_name": "Scholarly Showdown",
    "podcast_tagline": "Where Ideas Clash and Knowledge Emerges",
    "engagement_techniques": [
        "socratic questioning",
        "historical references",
        "thought experiments"
    ],  # Techniques to stimulate critical thinking
    "creativity": 0  # Low creativity to maintain focus on facts and logic
}
```

### Storytelling Adventure

This configuration turns the podcast into an interactive storytelling experience, engaging the audience in a narrative journey. It's ideal for fiction podcasts or creative content marketing.

```yaml
word_count: 1000  # Shorter to maintain pace and suspense
conversation_style: 
  - narrative
  - suspenseful
  - descriptive  # Creates an immersive story experience
roles_person1: storyteller
roles_person2: audience participator  # Allows for interactive elements
dialogue_structure: 
  - Scene Setting
  - Character Introduction
  - Rising Action
  - Climax
  - Resolution  # Follows classic storytelling structure
podcast_name: Tale Spinners
podcast_tagline: Where Every Episode is an Adventure
engagement_techniques: 
  - cliffhangers
  - vivid imagery
  - audience prompts  # Keeps the audience engaged and coming back
creativity: 0.9 # High creativity for unique and captivating stories
```

## Customization Scenarios

### Using the Python Package

When using the Podcastfy Python package, you can customize the conversation by passing a dictionary to the `conversation_config` parameter:

```python
from podcastfy.client import generate_podcast

custom_config = {
    "word_count": 200,
    "conversation_style": ["casual", "humorous"],
    "podcast_name": "Tech Chuckles",
    "creativity": 7
}

generate_podcast(
    urls=["https://example.com/tech-news"],
    conversation_config=custom_config
)
```

### Using the CLI

When using the Podcastfy CLI, you can specify a path to a YAML file containing your custom configuration:

```bash
podcastfy --url https://example.com/tech-news --conversation-config path/to/custom_config.yaml
```

The `custom_config.yaml` file should contain your configuration in YAML format:

```yaml
word_count: 200
conversation_style: 
  - casual
  - humorous
podcast_name: Tech Chuckles
creativity: 7
```

### 3. Dev Config

For contributors to the Podcastfy package, the default configuration is stored in [podcastfy/conversation_config.yaml](https://github.com/souzatharsis/podcastfy/blob/main/podcastfy/conversation_config.yaml). This file serves as the baseline configuration for all generated podcasts.

To modify the default configuration:

1. Locate the `conversation_config.yaml` file in the project root.
2. Edit the file using your preferred text editor.
3. Commit and push your changes, justifying proposed changes.

Remember that changes to this file will affect the default behavior of Podcastfy for all users. Consider the impact of your changes and discuss significant modifications with the project maintainers before implementing them.


## Notes of Caution

- The `word_count` is a target, and the AI may generate more or less than the specified word count. Low word counts are more likely to generate high-level discussions, while high word counts are more likely to generate detailed discussions.
- The `output_language` defines both the language of the transcript and the language of the audio. Here's some relevant information:
  - Bottom-line: non-English transcripts are good enough but non-English audio is work-in-progress.
  - Transcripts are generated using Google's Gemini 1.5 Pro, which supports 100+ languages by default.
  - Audio is generated using `openai` (default) or `elevenlabs` TTS models. 
    - The `openai` TTS model supports multiple languages automatically, however non-English voices still present sub-par quality in my experience.
    - The `elevenlabs` TTS model has English voices by default, in order to use a non-English voice you would need to download a custom voice for the target language in your `elevenlabs` account settings and then set the `text_to_speech.elevenlabs.default_voices` parameters to the voice you want to use in the [config.yaml file](https://github.com/pedroslopez/podcastfy/blob/main/podcastfy/config.yaml) (this config file is only available in the source code of the project, not in the pip package, hence if you are using the pip package you will not be able to change the ElevenLabs voice). For more information on ElevenLabs voices, visit [ElevenLabs Voice Library](https://elevenlabs.io/voice-library)