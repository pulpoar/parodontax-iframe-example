# Parodontax Plaque Check - iFrame Integration Example

## Overview

This folder contains a simple HTML example demonstrating how to embed the Parodontax Plaque Check application into any website using an iFrame. This method allows for quick and easy integration of the application into external sites.

## How to Use

1.  Upload the `index.html` file from this folder to your own server.
2.  Navigate to the URL of this file in your browser.

The application will load within an iFrame, occupying the full page.

## Contents of `index.html`

### iFrame Structure

```html
<iframe
    src="https://plaque-check.pulpoar.com/"
    style="flex: 1; border: none; width: 100%; height: 100%"
    frameborder="0"
    allow="camera *;microphone *"
></iframe>
```

-   **`src`**: Specifies the live URL of the Plaque Check application.
-   **`style`**: Ensures the iFrame fits perfectly within its container (in this case, the `body`).
-   **`allow`**: Grants the application inside the iFrame permission to request access to hardware like the camera and microphone. This is mandatory for the AR (Augmented Reality) experience to function.

### Message Forwarding

```javascript
<script>
    window.addEventListener('message', function(event) {
    if (event.origin === 'https://booster.pulpoar.com') {
        // Forward the message to parent window
        window.parent.postMessage(event.data, '*');
    }
    });
</script>
```

This script listens for `postMessage` events originating from the Plaque Check application within the iFrame. If the event comes from a trusted source (`https://booster.pulpoar.com`), it forwards the event to the parent window (the site hosting the iFrame).

This mechanism allows the Plaque Check application to communicate with the site that hosts it. For example, it can be used to send information to the parent site when the scan is complete or when the user performs a specific action.