# Audio Rules

Audio Rules govern the import settings of audio clips. Each rule is opt-in: check it to enforce it, leave it unchecked to ignore that property entirely.

## Purpose

Keep load type, compression, and channel layout consistent across a folder, so sound effects, music, and ambience each import the way your project expects.

<p align="center">
  <img src="../images/screenshot-04-before-after.png" width="800" alt="An audio clip's import settings before and after a Conduit policy is applied">
</p>

The same audio clip's Inspector before a policy is applied (left) and after (right). In this example Load Type changes from Decompress On Load to Compressed In Memory, and Compression Format changes from Vorbis to PCM, to match the governing policy.

## Configuration

| Rule | Governs | Notes |
|---|---|---|
| Default Load Type | Load type | Decompress On Load, Compressed In Memory, or Streaming. |
| Preload Audio Data | Preload | Loads clip data at scene load rather than on first play. |
| Compression Format | Compression format | PCM, Vorbis, ADPCM, and the other formats Unity supports. |
| Compression Quality | Quality | 0 to 1. Only meaningful for compressed formats. |
| Sample Rate Setting | Sample rate mode | Preserve Sample Rate, Optimize Sample Rate, or Override Sample Rate. |
| Sample Rate Override | Sample rate | Only applied when Sample Rate Setting is Override Sample Rate. |
| Force To Mono | Channel layout | Downmixes stereo sources to mono. Common for sound effects, rarely wanted for music. |
| Load In Background | Background loading | Loads off the main thread. Reduces load-time hitches for large clips. |

### Per-platform overrides

An audio policy can also hold a list of per-platform overrides: Load Type, Compression Format, and Compression Quality, keyed by platform name (`iPhone`, `Android`, and so on, matching the platform tab names in Unity's Audio Importer). These apply on top of the default settings for that one platform, the same way Unity's own per-platform tabs work.

Platform overrides appear as a standard list in the policy inspector. There is no dedicated per-platform interface in 1.0.0, so add entries by platform name exactly as Unity's Audio Importer names them.

### Settings that are not covered, on purpose

Two settings in Unity's Audio Import Settings are deliberately not exposed as rules.

**Normalize.** Unity does not expose this as a public, documented setting. The only way to reach it is through an undocumented internal field that is not guaranteed to be stable across Unity versions, and setting it that way is known to reset Force To Mono. Applying it automatically across a folder is not safe.

**Ambisonic.** This flags a clip as 360-degree B-format surround audio. That is a fact about how the source was recorded, not an import convention. A folder policy cannot know whether a given clip is ambisonic, and applying the setting to an ordinary stereo or mono clip produces broken playback.

## Examples

A sound-effects folder commonly wants Force To Mono on and Compression Format set to Vorbis with a moderate quality value.

A music or ambience folder commonly wants Load Type set to Streaming and Load In Background on, so a long track does not cause a hitch when a scene loads.
