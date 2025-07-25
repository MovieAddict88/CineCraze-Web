# DRM License Configuration Guide for CineCraze

This guide explains how to add DRM (Digital Rights Management) license configuration to your JSON data for MPD (MPEG-DASH) streams. The DRM configuration will only be used during playback and **will not affect data fetching**.

## Overview

The CineCraze player supports DRM-protected MPD streams with the following DRM systems:
- **Widevine** (Google Chrome, Android, etc.)
- **PlayReady** (Microsoft Edge, Internet Explorer, etc.)
- **ClearKey** (For testing purposes)

## JSON Structure

You can add DRM configuration at different levels in your JSON data:

### 1. Movie/Live Content Level

For movies and live TV content, add the `drm` section to the content entry:

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
    }
  ],
  "drm": {
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

### 2. Series Level

For TV series, you can add DRM configuration at the series level (applies to all episodes):

```json
{
  "Title": "Sample Series",
  "Description": "A sample DRM-protected TV series",
  "Type": "Series",
  "Poster": "https://example.com/series-poster.jpg",
  "drm": {
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

You can also add DRM configuration to individual episodes (overrides series-level DRM):

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
  "drm": {
    "widevine": {
      "licenseUrl": "https://different-drm-server.example.com/AcquireLicense",
      "headers": {
        "X-Episode-Token": "special-token"
      }
    }
  }
}
```

## DRM Configuration Options

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

Here's a complete example of a JSON structure with DRM configuration:

```json
{
  "Categories": [
    {
      "MainCategory": "Movies",
      "Entries": [
        {
          "Title": "DRM Protected Movie",
          "Description": "A movie protected with DRM",
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
              "name": "1080p",
              "url": "https://example.com/movie.mpd"
            }
          ],
          "drm": {
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
          "Title": "DRM Protected Series",
          "Description": "A TV series protected with DRM",
          "Type": "Series",
          "Poster": "https://example.com/series-poster.jpg",
          "Year": "2024",
          "Rating": "9.0",
          "Country": "USA",
          "SubCategory": "Drama",
          "drm": {
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
                  "Title": "Episode 1",
                  "Description": "First episode",
                  "Servers": [
                    {
                      "name": "1080p",
                      "url": "https://example.com/s1e1.mpd"
                    }
                  ]
                },
                {
                  "Episode": 2,
                  "Title": "Special Episode",
                  "Description": "Episode with different DRM",
                  "Servers": [
                    {
                      "name": "1080p",
                      "url": "https://example.com/s1e2.mpd"
                    }
                  ],
                  "drm": {
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

1. **DRM Configuration is Optional**: If no DRM configuration is provided, the player will attempt to play the MPD stream without DRM protection.

2. **Priority**: Episode-level DRM configuration takes priority over series-level configuration.

3. **Browser Support**: 
   - Widevine works in Chrome, Firefox, Edge, and most Android browsers
   - PlayReady works in Edge and Internet Explorer on Windows

4. **HTTPS Required**: DRM-protected content requires HTTPS for both the content and license servers.

5. **CORS**: License servers must be configured to allow cross-origin requests from your domain.

6. **No Impact on Data Fetching**: The DRM configuration is only used during playback and does not affect how the JSON data is fetched or processed.

## Testing

You can test your DRM configuration by:

1. Adding the DRM section to your JSON data
2. Opening the content in the player
3. Checking the browser console for DRM-related messages
4. Verifying that the license requests are being made to your license servers

The player will log detailed information about DRM initialization and any errors that occur during the license acquisition process.