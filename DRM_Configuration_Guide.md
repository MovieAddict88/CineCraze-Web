# MPD DRM License Configuration Guide for CineCraze

This guide explains how to add DRM (Digital Rights Management) license configuration to your JSON data **specifically for MPD (MPEG-DASH) streams only**. The DRM configuration will only be used during MPD playback and **will not affect data fetching**.

## Overview

The CineCraze player supports DRM-protected MPD streams with the following DRM systems:
- **Widevine** (Google Chrome, Android, etc.)
- **PlayReady** (Microsoft Edge, Internet Explorer, etc.)
- **ClearKey** (For testing purposes)

**Important**: DRM configuration is **only applied to MPD (.mpd) files**. Other video formats (MP4, HLS, etc.) will ignore DRM settings.

## JSON Structure

You can add MPD DRM configuration at different levels in your JSON data using the `mpdDrm` field:

### 1. Movie/Live Content Level

For movies and live TV content with MPD streams, add the `mpdDrm` section:

```json
{
  "Title": "Sample Movie",
  "Description": "A sample DRM-protected movie",
  "Type": "Movie",
  "Poster": "https://example.com/poster.jpg",
  "Thumbnail": "https://example.com/thumb.jpg",
  "Year": "2024",
  "Rating": "8.5",
  "Duration": "120 min",
  "Country": "USA",
  "Servers": [
    {
      "name": "1080p",
      "url": "https://example.com/movie.mpd"
    },
    {
      "name": "720p",
      "url": "https://example.com/movie_720.mp4"
    }
  ],
  "mpdDrm": {
    "widevine": {
      "licenseUrl": "https://drm-widevine-licensing.example.com/AcquireLicense",
      "headers": {
        "X-Custom-Header": "custom-value",
        "Authorization": "Bearer your-token"
      },
      "audioRobustness": "SW_SECURE_CRYPTO",
      "videoRobustness": "SW_SECURE_DECODE"
    },
    "playready": {
      "licenseUrl": "https://drm-playready-licensing.example.com/AcquireLicense",
      "headers": {
        "X-Custom-Header": "custom-value"
      }
    }
  }
}
```

**Note**: In this example, DRM will only be applied when playing the `.mpd` file. The `.mp4` file will play normally without DRM.

### 2. Series Level

For TV series with MPD episodes, add DRM configuration at the series level (applies to all MPD episodes):

```json
{
  "Title": "Sample Series",
  "Description": "A sample DRM-protected TV series",
  "Type": "Series",
  "Poster": "https://example.com/series-poster.jpg",
  "mpdDrm": {
    "widevine": {
      "licenseUrl": "https://drm-widevine-licensing.example.com/AcquireLicense"
    }
  },
  "Seasons": [
    {
      "Season": 1,
      "SeasonPoster": "https://example.com/season1-poster.jpg",
      "Episodes": [
        {
          "Episode": 1,
          "Title": "Episode 1",
          "Description": "First episode",
          "Servers": [
            {
              "name": "1080p",
              "url": "https://example.com/s1e1.mpd"
            }
          ]
        }
      ]
    }
  ]
}
```

### 3. Episode Level

You can also add MPD DRM configuration to individual episodes (overrides series-level DRM):

```json
{
  "Episode": 1,
  "Title": "Special Episode",
  "Description": "This episode has different DRM settings",
  "Servers": [
    {
      "name": "1080p",
      "url": "https://example.com/special-episode.mpd"
    }
  ],
  "mpdDrm": {
    "widevine": {
      "licenseUrl": "https://different-drm-server.example.com/AcquireLicense",
      "headers": {
        "X-Episode-Token": "special-token"
      }
    }
  }
}
```

## MPD DRM Configuration Options

### Widevine Configuration

```json
"widevine": {
  "licenseUrl": "https://your-widevine-license-server.com/license",
  "headers": {
    "X-Custom-Header": "value",
    "Authorization": "Bearer token"
  },
  "audioRobustness": "SW_SECURE_CRYPTO",
  "videoRobustness": "SW_SECURE_DECODE"
}
```

**Options:**
- `licenseUrl` (required): The Widevine license server URL
- `headers` (optional): Custom HTTP headers for license requests
- `audioRobustness` (optional): Audio robustness level
  - `SW_SECURE_CRYPTO` (Level 3 - Software)
  - `SW_SECURE_DECODE` (Level 3 - Software)
  - `HW_SECURE_CRYPTO` (Level 1 - Hardware)
  - `HW_SECURE_ALL` (Level 1 - Hardware)
- `videoRobustness` (optional): Video robustness level (same options as audio)

### PlayReady Configuration

```json
"playready": {
  "licenseUrl": "https://your-playready-license-server.com/license",
  "headers": {
    "X-Custom-Header": "value"
  }
}
```

**Options:**
- `licenseUrl` (required): The PlayReady license server URL
- `headers` (optional): Custom HTTP headers for license requests

### ClearKey Configuration (For Testing)

```json
"clearkey": {
  "keys": {
    "key-id-1": "key-value-1",
    "key-id-2": "key-value-2"
  }
}
```

**Options:**
- `keys` (required): Object mapping key IDs to key values

## Complete Example

Here's a complete example with mixed content types where only MPD files use DRM:

```json
{
  "Categories": [
    {
      "MainCategory": "Movies",
      "Entries": [
        {
          "Title": "DRM Protected Movie",
          "Description": "A movie with both MPD and MP4 versions",
          "Type": "Movie",
          "Poster": "https://example.com/poster.jpg",
          "Thumbnail": "https://example.com/thumb.jpg",
          "Year": "2024",
          "Rating": "8.5",
          "Duration": "120 min",
          "Country": "USA",
          "SubCategory": "Action",
          "Servers": [
            {
              "name": "1080p DASH",
              "url": "https://example.com/movie.mpd"
            },
            {
              "name": "720p MP4",
              "url": "https://example.com/movie.mp4"
            }
          ],
          "mpdDrm": {
            "widevine": {
              "licenseUrl": "https://drm-widevine.example.com/license",
              "headers": {
                "Authorization": "Bearer your-auth-token"
              },
              "videoRobustness": "SW_SECURE_DECODE"
            },
            "playready": {
              "licenseUrl": "https://drm-playready.example.com/license"
            }
          }
        }
      ]
    },
    {
      "MainCategory": "TV Series",
      "Entries": [
        {
          "Title": "Mixed Format Series",
          "Description": "A TV series with different episode formats",
          "Type": "Series",
          "Poster": "https://example.com/series-poster.jpg",
          "Year": "2024",
          "Rating": "9.0",
          "Country": "USA",
          "SubCategory": "Drama",
          "mpdDrm": {
            "widevine": {
              "licenseUrl": "https://drm-widevine.example.com/license",
              "headers": {
                "X-Series-ID": "series-123"
              }
            }
          },
          "Seasons": [
            {
              "Season": 1,
              "SeasonPoster": "https://example.com/season1.jpg",
              "Episodes": [
                {
                  "Episode": 1,
                  "Title": "Episode 1 (MPD with DRM)",
                  "Description": "MPD episode with DRM protection",
                  "Servers": [
                    {
                      "name": "1080p",
                      "url": "https://example.com/s1e1.mpd"
                    }
                  ]
                },
                {
                  "Episode": 2,
                  "Title": "Episode 2 (MP4 without DRM)",
                  "Description": "MP4 episode without DRM",
                  "Servers": [
                    {
                      "name": "1080p",
                      "url": "https://example.com/s1e2.mp4"
                    }
                  ]
                },
                {
                  "Episode": 3,
                  "Title": "Special Episode (Custom DRM)",
                  "Description": "MPD episode with custom DRM",
                  "Servers": [
                    {
                      "name": "1080p",
                      "url": "https://example.com/s1e3.mpd"
                    }
                  ],
                  "mpdDrm": {
                    "widevine": {
                      "licenseUrl": "https://special-drm.example.com/license",
                      "headers": {
                        "X-Episode-ID": "special-episode"
                      }
                    }
                  }
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

## Important Notes

1. **MPD Files Only**: DRM configuration is **only applied to MPD (.mpd) files**. Other video formats will ignore DRM settings.

2. **Automatic Detection**: The player automatically detects MPD URLs and applies DRM configuration only when needed.

3. **Priority**: Episode-level `mpdDrm` configuration takes priority over series-level configuration.

4. **Browser Support**: 
   - Widevine works in Chrome, Firefox, Edge, and most Android browsers
   - PlayReady works in Edge and Internet Explorer on Windows

5. **HTTPS Required**: DRM-protected content requires HTTPS for both the content and license servers.

6. **CORS**: License servers must be configured to allow cross-origin requests from your domain.

7. **No Impact on Data Fetching**: The `mpdDrm` configuration is only used during MPD playback and does not affect how the JSON data is fetched or processed.

8. **Mixed Content Support**: You can have both MPD and non-MPD content in the same entry. DRM will only be applied to MPD streams.

## Testing

You can test your MPD DRM configuration by:

1. Adding the `mpdDrm` section to your JSON data for content with MPD URLs
2. Opening the content in the player
3. Selecting the MPD stream from the quality selector
4. Checking the browser console for DRM-related messages
5. Using the testing function: `testMPDDRMConfig(mpdDrmConfig, mpdUrl)`

## Console Testing Functions

- `checkDRMSupport()` - Check what DRM systems are supported by the browser
- `testMPDDRMConfig(config, mpdUrl)` - Test MPD DRM configuration (only works with .mpd URLs)
- `getPlayerState()` - Get current player and MPD DRM status

The player will log detailed information about DRM initialization and any errors that occur during the license acquisition process for MPD streams only.