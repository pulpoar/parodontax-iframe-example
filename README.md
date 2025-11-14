# Parodontax Plaque Check - SDK Integration Example

## Overview

This folder contains a simple HTML example demonstrating how to embed the Parodontax Plaque Check application into a website using an iFrame and communicate with it via the `@pulpoar/plugin-sdk`.

## How to Use

Open `index.html` directly in your web browser. The example is configured to point to a local development server (e.g. `http://localhost:5173/`). You can change the `src` attribute of the `<iframe>` to point to the live application URL.

## SDK Integration

The integration relies on two parts: the HTML file that hosts the iframe, and the SDK script that facilitates communication.

### 1. Iframe Setup

The application is embedded using a standard `<iframe>` element. The `allow="camera *;"` attribute is mandatory for the experience to function correctly.

```html
<iframe
    src="https://plaque-check.pulpoar.com/"
    style="flex: 1; border: none; width: 100%; height: 100%"
    frameborder="0"
    allow="camera *;"
></iframe>
```

### 2. Loading the SDK

The SDK is loaded via a `<script>` tag. It exposes a global `pulpoar` object that provides methods for interacting with the iframe.

```html
<script src="https://cdn.jsdelivr.net/npm/@pulpoar/plugin-sdk@latest/dist/index.iife.js"></script>
```

## Listening for Events (From Iframe)

The SDK allows you to listen for various events emitted by the Plaque Check application. You can subscribe to these events using the `pulpoar.on<EventName>()` methods.

**Example:**
```javascript
pulpoar.onReady((payload) => {
  console.log('Application is ready:', payload);
});

pulpoar.onPathChange((payload) => {
  console.log('Path changed:', payload);
});
```

### Available Events

| Event Name                    | Payload                                                               | Description                                                                 |
| ----------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| `onReady`                     | `{ ready: boolean }`                                                  | Triggers when the application is initialized and ready to receive actions.  |
| `onPathChange`                | `{ path: string, referer: string }`                                   | Triggers when the user navigates between pages within the application.      |
| `onStartExperience`           | `undefined`                                                           | Triggers when the user clicks the "Başlat" button on the welcome screen.    |
| `onStartScanning`             | `undefined`                                                           | Triggers when the user clicks the "Taramayı Başlat" button on the introduction page. |
| `onTakePhoto`                 | `{ photoTaken: boolean }`                                             | Triggers when the camera automatically captures a photo for analysis.       |
| `onGdprApprove`               | `{ approved: boolean }`                                               | Triggers when the user approves the GDPR consent checkbox.                  |
| `onHamburger`                 | `{ from: string, to: string }`                                        | Triggers when a link in the hamburger menu is clicked.                      |
| `onOpenCamera`                | `undefined`                                                           | Triggers when the user clicks the "Kamerayı Aç" button on the GDPR sheet.   |
| `onAcceptPhoto`               | `undefined`                                                           | Triggers when the user clicks "Devam Et" to accept the captured photo.      |
| `onRetakePhoto`               | `undefined`                                                           | Triggers when the user clicks "Yeniden Çek" to retake the photo.          |
| `onResultPage`                | `{ plaqueRatio: number, recommendedProducts: Product[] }`             | Fires when the result page is loaded, containing the analysis score and recommended products. |
| `onGoToRecommendationDetails` | `{ products: Product[] }`                                             | Triggers when the "İncele" button on the result page is clicked.          |
| `onGoToProduct`               | `{ product: Product, isRecommendedProduct: boolean }`                 | Triggers when a product card on the final `finish` page is clicked.         |
| `onError`                     | `{ message: string }`                                                 | Fires when a critical error occurs (e.g., camera permission denied, WASM load failure). |

*The `Product` object has the following structure: `{ id: string, name: string, link: string, image_url: string, ean: string }`*
