# Sber SaluteSpeech Plugin for TypeWhisper

External TypeWhisper transcription engine plugin for the Sber SaluteSpeech REST API.

## Features

- OAuth with a SaluteSpeech Authorization Key.
- Personal and corporate scopes: `SALUTE_SPEECH_PERS`, `SALUTE_SPEECH_CORP`.
- Short recordings use synchronous recognition: `/rest/v1/speech:recognize`.
- Longer recordings use async upload/task/download recognition.
- Audio is sent as 16 kHz mono `PCM_S16LE`.
- Credentials are stored by TypeWhisper in the plugin-scoped Keychain.

## Credentials

Create a SaluteSpeech project and generate an Authorization Key:

```text
Base64(Client ID:Client Secret)
```

Paste that value into the plugin settings. Do not prefix it with `Basic`; the plugin adds that header value when requesting OAuth tokens.

## Install For Development

Build the `SaluteSpeechPlugin` bundle from the TypeWhisper checkout, then copy it to the dev plugin folder:

```bash
xcodebuild -project TypeWhisper.xcodeproj -target SaluteSpeechPlugin -configuration Debug build

mkdir -p "$HOME/Library/Application Support/TypeWhisper-Dev/Plugins"
rm -rf "$HOME/Library/Application Support/TypeWhisper-Dev/Plugins/SaluteSpeechPlugin.bundle"
cp -R build/Debug/SaluteSpeechPlugin.bundle "$HOME/Library/Application Support/TypeWhisper-Dev/Plugins/"
```

Restart TypeWhisper Dev and enable `Sber SaluteSpeech` in `Settings -> Plugins -> My Plugins`.

## Install For Release App

Copy the built bundle to:

```text
~/Library/Application Support/TypeWhisper/Plugins/
```

## Repository Layout

- `SaluteSpeechPlugin.swift` - plugin implementation.
- `manifest.json` - TypeWhisper plugin manifest.
- `Tests/SaluteSpeechPluginTests.swift` - request/response unit tests.

## Notes

The plugin is a batch transcription engine. Transcript preview fallback is disabled to avoid sending repeated partial recordings to SaluteSpeech during a single dictation session.
