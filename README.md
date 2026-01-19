# Liveness SDK - Webpack Build Guide

This guide explains how to use the Liveness SDK and browser compatibility.

## Release Notes

-

## Usage in HTML

### Method 1: Script Tag (Recommended)

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Liveness SDK Example</title>
  </head>
  <body>
    <div id="liveness-container"></div>

    <!-- Include the bundled SDK -->
    <script src="./versions/<version>/liveness-sdk.min.js"></script>

    <script>
      // Liveness is now available as a global constructor
      const liveness = new Liveness({
        container: document.getElementById("liveness-container"),
        wsUrl: "ws://localhost:8000",
        width: 640,
        height: 480,
        successCallback: (result) => {
          console.log("Success:", result);
        },
        errorCallback: (error) => {
          console.error("Error:", error);
        },
      });

      // Set token and start
      liveness.setToken("your-token-here");
      liveness.warmUp().then(() => {
        liveness.start();
      });
    </script>
  </body>
</html>
```

### Method 2: ES6 Module Import (if using a module bundler)

```javascript
import Liveness from "./versions/<version>/liveness-sdk.min.js";

const liveness = new Liveness({
  // configuration options
});
```

## Configuration Options

The Liveness constructor accepts the following options:

```javascript
{
    container: HTMLElement,        // Container element for the liveness UI
    wsUrl: string,                // WebSocket server URL
    width: number,                // Canvas width (optional)
    height: number,               // Canvas height (optional)
    successCallback: function,    // Called when liveness detection succeeds
    errorCallback: function,      // Called when an error occurs
    layout: {                     // UI customization (optional)
        colors: {
            primary: string,
            secondary: string,
            background: string,
            text: string
        }
    }
}
```

## API Methods

- `setToken(token)` - Set authentication token
- `warmUp()` - Initialize camera and UI (returns Promise)
- `start()` - Start liveness detection
- `pause()` - Pause detection
- `resume()` - Resume detection
- `restart()` - Restart detection
- `unmountLiveness()` - Clean up and destroy instance

## Troubleshooting

### "Liveness is not a constructor" Error

This error typically occurs when:

1. **Incorrect import**: Use `<script src="./versions/<version>/liveness-sdk.min.js"></script>` for browser usage

### Camera Access Issues

- Ensure your page is served over HTTPS (required for camera access)
- Check browser permissions for camera access
- Verify the container element exists before initializing

### WebSocket Connection Issues

- Verify the WebSocket URL is correct
- Ensure the WebSocket server is running
- Check for CORS issues if connecting to a different domain
