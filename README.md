<img align="right" src="https://i.imgur.com/zrE80HY.png" height="200" width="200">

# JMusicBot (TrueOG Network fork)

A self-hosted Discord music bot, forked by the [TrueOG Network](https://true-og.net) from [chrisb09/MusicBot](https://github.com/chrisb09/MusicBot), which is in turn a modernised fork of [jagrosh/MusicBot](https://github.com/jagrosh/MusicBot).

The upstream fork keeps the bot working on current Discord, JDA, and YouTube; this fork keeps it running on TrueOG's infrastructure.

## TrueOG Network Fork Changes

- Maven Wrapper (`./mvnw`) so builds use a pinned Maven version instead of whatever is installed
- Selenium bumped to 4.49.0 with the CDP v153 devtools module, so YouTube PO-token extraction works with current Chromium
- `maven-shade-plugin` now merges `META-INF/services`, so bundled `CdpInfo` (and other SPI) registrations are no longer silently dropped from the fat jar
- YouTube PO-token capture handles the gzip-compressed `/youtubei/v1/player` request body that YouTube web client 2.20260918+ sends, and logs a warning instead of silently timing out when the body cannot be parsed

## Inherited From Upstream (chrisb09)

- JDA 6 support and current dependency updates
- DAVE voice encryption support for current Discord voice requirements
- Optional YouTube account login, PO token, and visitor-data support
- Optional per-channel playback status message
- Optional JDBC/SQLite playback analytics logging
- Regular [lavaplayer](https://mvnrepository.com/artifact/dev.arbjerg/lavaplayer)/[lavalink](https://maven.lavalink.dev/#/releases/dev/lavalink/youtube/common) bumps

## Quick Links

- Setup guide: [docs/setup.md](/docs/setup.md)
- Upstream fork changelog: [docs/fork-changelog.md](/docs/fork-changelog.md)

## Features

- Easy self-hosting
- Fast song loading
- Smooth playback
- DJ role and server-specific settings
- Queue management and playlist support
- Support for many sources, streams, and local files

## Supported Sources And Formats

JMusicBot supports all sources and formats supported by [lavaplayer](https://github.com/sedmelluq/lavaplayer#supported-formats).

Sources:
- YouTube
- SoundCloud
- Bandcamp
- Vimeo
- Twitch streams
- Local files
- HTTP URLs

Formats:
- MP3
- FLAC
- WAV
- Matroska/WebM
- MP4/M4A
- OGG streams
- AAC streams
- Stream playlists such as M3U and PLS

## Building

```bash
./mvnw clean package
```

The runnable jar is written to `target/JMusicBot-<version>-All.jar`.

## Setup Notes

1. Create a Discord bot and get its token, see [here](https://jmusicbot.com/getting-a-bot-token/).
2. Enable the `MESSAGE_CONTENT` intent in the Discord Developer Portal [here](https://discord.com/developers/applications) ([Wiki link](https://jda.wiki/using-jda/gateway-intents-and-member-cache-policy/)).
3. Configure `config.txt`.
4. Run the jar: `java -Dnogui=true -jar target/JMusicBot-<version>-All.jar`

For the detailed version, see [docs/setup.md](/docs/setup.md).

## YouTube Playback

YouTube requires a PO token, which the bot obtains at startup by driving a headless Chromium through Selenium. Chromium must be installed, and the bundled Selenium CDP module must match the installed Chromium major version (currently v153 — see `pom.xml`). If Chromium is upgraded past that, bump `selenium-java` and the `selenium-devtools-vNNN` exclusions in `pom.xml` and rebuild.

Google account login is also supported as a fallback:

```conf
youtubeoauth2=true
```

Do not use your main Google account. Use a burner or otherwise disposable account instead. Details are in [docs/setup.md](/docs/setup.md).

## DAVE / Voice Encryption

Discord requires DAVE for voice on modern API versions. The bot configures `libdave-jvm` during startup and bundles native binaries for common Linux, Windows, and macOS targets.

If you deploy on an unsupported architecture, add the matching native dependency in [pom.xml](/pom.xml).

## Upstream And Related Work

This fork incorporates ideas and patches related to:

- [PR-1670](https://github.com/jagrosh/MusicBot/pull/1670)
- [PR-1703](https://github.com/jagrosh/MusicBot/pull/1703)
- [PR-1772](https://github.com/jagrosh/MusicBot/pull/1772)
