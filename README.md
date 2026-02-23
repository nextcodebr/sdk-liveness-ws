# Detection SDK

This guide explains how to use the Detection SDK and browser compatibility.

## Release Notes

### v0.1.15 (2026-02-23)
- SDK and Detection statuses

### v0.1.14 (2026-02-19)
- Response status success|failed|waiting

### v0.1.13 (2026-02-13)
- performance improvement

### v0.1.12 (2026-02-03)
- fix README.md
-  fix sdk build

### v0.1.11 (2026-02-03)
- fix README.md

### v0.1.10 (2026-02-03)
- testing release notes

### v0.1.9 (2026-02-03)
- add new build method


## Usage in HTML

You can use the `detection-sdk.min.js` as Document or Liveness detection, depending on the `style` parameter.

### Method 1: Script Tag (Recommended)

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Detection SDK Example</title>
  </head>
  <body>
    <div id="sdk-container"></div>

    <!-- Include the bundled SDK -->
    <script src="https://cdn.jsdelivr.net/gh/nextcodebr/detection-sdk-ws/versions/*version*/detection-sdk.min.js"></script>

    <script>
      // Detection is now available as a global constructor
      const detection = new DetectionSDK({
        container: document.getElementById("sdk-container"),
        style: "normal", // Liveness style (normal | c6 | facetech | document)
        wsUrl: "wss://detection-sdk-ws.nxcd.app:8000",
        width: 640,
        height: 480,
        onDetectionResponse: (result) => {
          // result: { status: "DETECTION_SUCCESS" | "DETECTION_FAILED", photo: string, message: string }
          console.log("Detection result:", result);
        },
        onSDKStateChange: (state) => {
          // state: { status: "SDK_INITIALIZED" | "SDK_CAMERA_READY" | "SDK_WAITING" | "SDK_ERROR" | "DETECTION_STARTED", message?: string }
          console.log("SDK state:", state);
        },
      });

      // Set token and start
      detection.setToken("your-token-here");
      detection.warmUp().then(() => {
        detection.start();
      });
    </script>
  </body>
</html>
```

### Method 2: ES6 Module Import (if using a module bundler)

```javascript
import DetectionSDK from "https://cdn.jsdelivr.net/gh/nextcodebr/detection-sdk-ws/versions/*version*/detection-sdk.min.js";

const detection = new DetectionSDK({
  // configuration options
});
```

## Configuration Options

The DetectionSDK constructor accepts the following options:

```javascript
{
    container: HTMLElement,             // Container element for the Detection UI. (optional) Defaults to document.body.
    wsUrl: string,                      // WebSocket server URL
    width: number,                      // Canvas width (optional)
    height: number,                     // Canvas height (optional)
    style: string,                      // Liveness style: "normal" | "c6" | "facetech" | "document"
    onDetectionResponse: function,      // Called when detection finishes (success or failure).
    onSDKStateChange: function,         // Called on SDK lifecycle state changes.
    backButtonFunctionCallback: function, // Called when back button is pressed. (optional)
    layout: {                           // UI customization (optional)
        colors: {
            primary: string,
            secondary: string,
            background: string,
            text: string
        }
    },
}
```

## Enums

### `DetectionSDK.DetectionStatus`

| Key | Value | Description |
|---|---|---|
| `SUCCESS` | `"DETECTION_SUCCESS"` | Detection completed successfully |
| `FAILED` | `"DETECTION_FAILED"` | Detection completed but validation failed |

### `DetectionSDK.SDKState`

| Key | Value | Description |
|---|---|---|
| `INITIALIZED` | `"SDK_INITIALIZED"` | SDK warmUp completed |
| `CAMERA_READY` | `"SDK_CAMERA_READY"` | Camera permission granted and active |
| `WAITING` | `"SDK_WAITING"` | Photo is being processed |
| `ERROR` | `"SDK_ERROR"` | SDK error (camera denied, WebSocket failed) |
| `DETECTION_STARTED` | `"DETECTION_STARTED"` | Detection started analyzing frames |

### Callback Contracts

**`onDetectionResponse(result)`**
```javascript
{
  status: "DETECTION_SUCCESS" | "DETECTION_FAILED",
  photo: string,    // base64 jpeg
  message: string,  // descriptive message
}
```

**`onSDKStateChange(state)`**
```javascript
{
  status: "SDK_INITIALIZED" | "SDK_CAMERA_READY" | "SDK_WAITING" | "SDK_ERROR" | "DETECTION_STARTED",
  message: string | undefined,  // present for SDK_ERROR and SDK_WAITING
}
```

## API Methods

- `setToken(token: string)` - Set authentication JWT token
- `warmUp()` - Initialize camera and UI (returns Promise)
- `start()` - Start detection
- `pause()` - Pause detection
- `resume()` - Resume detection
- `restart()` - Restart detection
- `unmountDetectionSDK()` - Clean up and destroy instance

## Troubleshooting

### "DetectionSDK is not a constructor" Error

This error typically occurs when:

- **Incorrect import**: Use <script src="https://cdn.jsdelivr.net/gh/nextcodebr/detection-sdk-ws/versions/*version*/detection-sdk.min.js"></script> for browser usage

### Camera Access Issues

- Ensure your page is served over HTTPS (required for camera access)
- Check browser permissions for camera access
- Verify the container element exists before initializing (leaving it empty the SDK will use html body as container)

### WebSocket Connection Issues

- Verify the WebSocket URL is correct
