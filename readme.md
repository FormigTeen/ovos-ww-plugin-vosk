## Description
Mycroft wake word plugin for [Vosk](https://alphacephei.com/vosk/)
with Active Wake Word Detect by Bus

## Install

`pip install git+https://github.com/FormigTeen/ovos-ww-plugin-active-vosk

## Configuration

### Quick start

Add the following to your hotwords section in mycroft.conf 

```json
  "listener": {
    "wake_word": "hey_computer"
  },
  "hotwords": {
    "hey_computer": {
        "module": "ovos-ww-plugin-active-vosk",
        "listen": true
    }
  }
```
replace `hey_computer` with your wake word and thats all!

a model wil be automatically downloaded for configured language

Some wake words are hard to trigger, usually if missing from the language model, 
eg, `hey mycroft` is usually transcribed as `hey microsoft`, 
by default this plugin will check for the wake word name, but the keyword can be configured in a number of ways

- `model_folder`- full path to a vosk model, optional, will be automatically downloaded
- `lang` - lang code for model, optional, will use global value if not set. only used to download models
- `debug` - if true will print extra info, like the transcription contents
- `rule` - how to process the transcript for detections, see examples below
- `time_between_checks` - the length in seconds between inferences, must be between 0.2 and 5
- `full_vocab` - use the full model vocabulary for transcriptions, if false (default) vosk will run in keyword mode
- `samples` - list of samples to match the rules against, optional, by default uses keyword name
- `event_names` - list of event names to enable wake word

```json
  "listener": {
    "wake_word": "hey_computer"
  },
  "hotwords": {
    "hey_computer": {
        "module": "ovos-ww-plugin-vosk",
        "listen": true,
        "full_vocab": true,
        "rule": "equals",
        "debug": true,
        "samples": ["hey computer", "a computer", "hey computed"],
        "model_folder": "/home/user/Downloads/vosk-model-small-en-us-0.4",
        "time_between_checks": 0.6,
        "event_names": ["enable.wake-word"]
    }
  }
```

#### Keyword Rules

You can define different rules to trigger a wake word

- `contains` - if the transcript contains any of provided samples 
- `equals` - if the transcript exactly matches any of provided samples 
- `starts` - if the transcript starts with any of provided samples 
- `ends` - if the transcript ends with any of provided samples 
- `fuzzy` - fuzzy match transcript against samples


TIP: enable `debug` flag and check logs for what is being transcribed, then finetune the rule and samples

Each wake word must fit in 3 seconds, which is the length of audio the model parses at a time

You can try to improve performance by tweaking `time_between_checks`, Lower values will decrease performance, higher values will decrease accuracy, default value is 1.0

set `full_vocab` to transcribe all known words before applying detection rules, by default this is false and the plugin will only look for the wake word samples, depending on wake word this may improve or decrease accuracy