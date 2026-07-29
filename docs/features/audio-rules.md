# Audio Rules

Audio Rules govern `AudioImporter` settings. Each rule is opt-in — check it to enforce it, leave it unchecked to ignore that property entirely.

<p align="center">
  <img src="../images/screenshot-04-before-after.png" width="800" alt="Audio import settings before and after a Conduit policy is applied">
</p>

Shows the same audio clip's Inspector before a policy is applied (left) and after (right). Force To Mono, Load Type, Compression Format, and Sample Rate Setting all update to match the governing policy, and the file's imported size drops accordingly.

| Rule | Governs | Notes |
|---|---|---|
| Default Load Type | `defaultSampleSettings.loadType` | Decompress On Load, Compressed In Memory, or Streaming. |
| Preload Audio Data | `defaultSampleSettings.preloadAudioData` | Loads clip data at scene load rather than on first play. |
| Compression Format | `defaultSampleSettings.compressionFormat` | PCM, Vorbis, ADPCM, and the other formats Unity supports. |
| Compression Quality | `defaultSampleSettings.quality` | 0–1. Only meaningful for compressed formats. |
| Sample Rate Setting | `defaultSampleSettings.sampleRateSetting` | Preserve Sample Rate, Optimize Sample Rate, or Override Sample Rate. |
| Sample Rate Override | `defaultSampleSettings.sampleRateOverride` | Only applied when Sample Rate Setting is Override Sample Rate. |
| Force To Mono | `forceToMono` | Downmixes stereo sources to mono. Common for SFX; rarely wanted for music. |
| Load In Background | `loadInBackground` | Loads off the main thread. Reduces load-time hitches for large clips. |

## Not covered, on purpose

Two settings visible in Unity's own Audio Import Settings inspector are deliberately not exposed as policy rules:

**Normalize.** Unity does not expose this as a public, documented property on `AudioImporter`. The only way to reach it is by reflecting into Unity's internal serialized field name, which is undocumented, not guaranteed stable across Unity versions, and has a known side effect of resetting Force To Mono when set that way. Applying it automatically across a folder isn't safe.

**Ambisonic.** This flags a clip as 360° B-format surround audio — a fact about how the source was recorded, not an import convention. A folder policy can't know whether a given clip actually is ambisonic content; applying this setting to an ordinary stereo or mono clip produces broken playback.

## Per-platform overrides

Beyond the default settings above, an audio policy can hold a list of per-platform overrides (Load Type, Compression Format, and Compression Quality), keyed by platform name — `iPhone`, `Android`, and so on, matching Unity's own platform tab names. These apply on top of the default settings for that one platform, the same way Unity's own per-platform tabs work. Platform overrides appear as a standard list in the policy inspector; there's no dedicated per-platform UI yet, so add entries by platform name exactly as Unity's Audio Importer names them.

## A typical setup

An SFX folder commonly wants Force To Mono on and Compression Format set to Vorbis with a moderate quality value. A music or ambience folder commonly wants Load Type set to Streaming and Load In Background on, so a long track doesn't cause a hitch when a scene loads.
