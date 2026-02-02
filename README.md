# Detection SDK - Webpack Build Guide

This guide explains how to use the Detection SDK and browser compatibility.

## Release Notes

-

## Usage in HTML

You can use the `detection-sdk.min.js` as Document or Liveness detection, depending on the `functionName` parameter.

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
    <script src="https://cdn.jsdelivr.net/gh/nextcodebr/detection-sdk-ws/versions/v0.1.3/detection-sdk.min.js"></script>

    <script>
      // Detection is now available as a global constructor
      const detection = new DetectionSDK({
        container: document.getElementById("sdk-container"),
        functionName: "Liveness", // Function name for the detection (Liveness | Document). Default is Liveness.
        wsUrl: "wss://detection-sdk-ws.nxcd.app:8000",
        width: 640,
        height: 480,
        onResponseCallback: (result) => {
          console.log("Response:", result);
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
import DetectionSDK from "https://cdn.jsdelivr.net/gh/nextcodebr/detection-sdk-ws/versions/v0.1.3/detection-sdk.min.js";

const detection = new DetectionSDK({
  // configuration options
});
```

## Configuration Options

The DetectionSDK constructor accepts the following options:

```javascript
{
    container: HTMLElement,       // Container element for the Detection UI. (optional) If not provided, the SDK will use body element.
    wsUrl: string,                // WebSocket server URL
    width: number,                // Canvas width (optional)
    height: number,               // Canvas height (optional)
    onResponseCallback: function, // Called when detection finishes. Receive an object with the detection result.
    functionName: string,         // Function name for the detection (Liveness | Document). Default is Liveness.
    layout: {                     // UI customization (optional)
        colors: {
            primary: string,
            secondary: string,
            background: string,
            text: string
        }
    },
}
```

## API Methods

- `setToken(token: string)` - Set authentication JWT token
- `warmUp()` - Initialize camera and UI (returns Promise)
- `start()` - Start detection
- `pause()` - Pause detection
- `resume()` - Resume detection
- `restart()` - Restart detection
- `setFunctionName(functionName: string)` - Set the function name for the detection (Liveness | Document). Default is Liveness.
- `unmountDetectionSDK()` - Clean up and destroy instance

## Troubleshooting

### "DetectionSDK is not a constructor" Error

This error typically occurs when:

- **Incorrect import**: Use <script src="https://cdn.jsdelivr.net/gh/nextcodebr/detection-sdk-ws/versions/<version>/detection-sdk.min.js"></script> for browser usage

### Camera Access Issues

- Ensure your page is served over HTTPS (required for camera access)
- Check browser permissions for camera access
- Verify the container element exists before initializing (leaving it empty the SDK will use html body as container)

### WebSocket Connection Issues

- Verify the WebSocket URL is correct
