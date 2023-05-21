# MIDI-LLM-tokenizer
Tools for converting .mid files into text for training large language models.
The exact format of the intermediary text is not critical. The primary goals are to 1. create a "legible" text format that can be easily used with existing LLM tech, and 2. encode midi data into an efficient format for training.

Expected workflow would be:
1. [convert midi datasets into jsonl files](#midi_to_jsonlpy)
2. [tokenize](#build_tokenizerpy) jsonl files into binidx
3. train LLM
4. sample LLM
5. [convert text into midi](#str_to_midipy)

For converting jsonl files to binidx format for training, see https://github.com/Abel2076/json2binidx_tool

# Vocabulary

MIDI files contain a lot of data, and only some of it can be reasonably learned by a language model.
Inspired by MuseNet and Oore et. al, 2018, we have two main types of tokens:
- Wait tokens for timing (125 of them)
- Combined note+velocity+instrument tokens (128 notes * 16 quantized velocity * 16 binned instruments = 32768)
And also pad/start/end tokens.

# Scripts

## build_tokenizer.py
Builds a new huggingface tokenizer and vocab using vocab_config.json

```sh
python ./build_tokenizer.py
```

## midi_to_jsonl.py
Converts a directory or archive of mid/midi files into a jsonl file of note sequences.

```sh
python ./midi_to_jsonl.py --path ~/lmd_full.tar.gz --output lmd_full.jsonl
```

## str_to_midi.py
Converts a note sequence in string format into a .mid file.

```sh
python ./str_to_midi.py "<start>p:15:C4 T10 p:0:C4 <end>" --output test.mid
```
