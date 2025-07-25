# DRM Configuration Examples

This directory contains example JSON files demonstrating how to configure DRM for MPD streams in CineCraze.

## Files

### `sample_drm_data.json`
Complete example showing various DRM configuration scenarios:
- Single MPD server with ClearKey DRM
- Mixed server types (MPD with DRM + HLS without DRM)
- Series-level DRM configuration
- Episode-level DRM overrides

## Important Notes

⚠️ **These are examples only** - Replace with your actual:
- Content URLs
- License server URLs  
- Key IDs and keys
- Authentication tokens

🔒 **Security**: Never commit real DRM keys to public repositories

📖 **Documentation**: See `DRM_Configuration_Guide.md` for complete documentation

## Usage

1. Copy the `mpdDrm` sections from these examples
2. Replace placeholder values with your actual DRM configuration
3. Add to your production JSON data
4. Test with the browser console functions

## Testing

Use these console functions to test your DRM configuration:
- `checkDRMSupport()` - Check browser DRM support
- `testMPDDRMConfig(config, mpdUrl)` - Test specific configurations
- `getPlayerState()` - Check current DRM status