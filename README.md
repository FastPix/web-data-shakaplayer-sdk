# Introduction

This SDK simplifies integration steps with [Shaka Player](https://github.com/shaka-project/shaka-player), enabling the collection of player analytics. It enables automatic tracking of video performance metrics, making the data readily available on the [FastPix dashboard](https://dashbord.fastpix.io) for monitoring and analysis. While the SDK is developed in TypeScript, the published npm package currently includes only the JavaScript output. TypeScript support, including type definitions, will be released in a future version.

# Key Features:

- **User Engagement Metrics:** Capture detailed viewer interaction data.
- **Playback Quality Monitoring:** Real-time performance analysis of your video streams.
- **Web Performance Insights:** Identify and resolve bottlenecks affecting video delivery.
- **Customizable Tracking:** Flexible configuration to match your specific monitoring needs.
- **Error Management:** Robust error handling and reporting.
- **Streaming Diagnostics:** Gain deep insights into the performance of your video streaming.

# Prerequisites:

## Getting started with FastPix:

To track and analyze video performance, initialize the FastPix Data SDK with your Workspace key:

1. **[Access the FastPix Dashboard](https://dashbord.fastpix.io)**: Log in and navigate to the Workspaces section.
2. **Locate Your Workspace Key**: Copy the Workspace Key for client-side monitoring. Include this key in your JavaScript code on every page where you want to track video performance.

# Installation:

To get started with the SDK, install using npm or your favourite node package manager 😉:

```bash
npm i @fastpix/data-shakaplayer
```

# Basic Usage:

## Import

```javascript
import loadShakaPlayer from "@fastpix/data-shakaplayer";
```

## Usage

`workspace_id` is the required field during initialization, you can also include other optional metadata to customize your tracking. For a comprehensive list of all the additional metadata options supported by the FastPix Data SDK, refer to the [User Passable Metadata](https://docs.fastpix.io/docs/user-passable-metadata) section.

Here's how you can quickly integrate FastPix Data SDK into your application to track and analyze video performance with ease:

### React

Integrate the following React code into your application to configure Shaka Player with FastPix.

```jsx
import React, { useEffect, useRef } from "react";
import loadShakaPlayer from "@fastpix/data-shakaplayer";
import shaka from "shaka-player"; // Import Shaka Player

const ShakaPlayer = () => {
  const videoElementRef = useRef(null); // Ref to the video element

  useEffect(() => {
    // Ensure Shaka Player is initialized and FastPix data is configured when the component mounts

    // Initialize player setup
    const initTime = loadShakaPlayer.utilityMethods.now(); // Captures the player initialization time
    const videoElement = videoElementRef.current; // Access video element through ref
    const player = new shaka.Player(videoElement); // Shaka Player instance bound to the video element

    // Define player metadata
    const playerMetadata = {
      workspace_id: "WORKSPACE_KEY", // Replace with your workspace key
      player_name: "PLAYER_NAME", // A unique identifier for the player instance
      player_init_time: initTime, // The time the player was initialized

      // Additional metadata can be added here
    };

    // Configure FastPix data integration
    const fastPixShakaIntegration = loadShakaPlayer(
      player,
      {
        debug: false, // Set to true to enable debug logs
        data: playerMetadata, // Pass metadata object
      },
      shaka, // The Shaka Player instance (mandatory field)
    );

    // Load the video content
    const videoUrl =
      "https://stream.fastpix.io/027a90e4-f5e2-433d-81e5-b99ee864c3f6.m3u8"; // Replace with your video manifest URL

    player
      .load(videoUrl) // Load the video manifest into the Shaka Player
      .then(() => {
        // Successfully loaded the manifest. FastPix data will begin tracking.
      })
      .catch((error) => {
        fastPixShakaIntegration.handleLoadError(error);
      });

    // Cleanup: Destroy the player instance and stop FastPix data tracking when the component unmounts
    return () => {
      player.fp.destroy(); // Ends FastPix tracking
      player.destroy(); // Destroys the Shaka Player instance
    };
  }, []); // Empty dependency array ensures this runs only once when the component mounts

  return (
    <div>
      <video ref={videoElementRef} id="video-player" width="100%" controls></video>
    </div>
  );
};

export default ShakaPlayer;
```

### JavaScript

Include the following JavaScript code in your application to set up the Shaka Player with FastPix:

```javascript
import loadShakaPlayer from "@fastpix/data-shakaplayer";
import shaka from "shaka-player"; // Import Shaka Player

// Initialize player setup
const initTime = loadShakaPlayer.utilityMethods.now(); // Captures the player initialization time
const videoElement = document.getElementById("video-player"); // HTML5 video element for Shaka Player
const player = new shaka.Player(videoElement); // Shaka Player instance bound to the video element

// Define player metadata
const playerMetadata = {
  workspace_id: "WORKSPACE_KEY", // Replace with your workspace key
  player_name: "PLAYER_NAME", // A unique identifier for the player instance
  player_init_time: initTime, // The time the player was initialized

  // Additional metadata can be added here
};

// Configure FastPix data integration
const fastPixShakaIntegration = loadShakaPlayer(
  player,
  {
    debug: false, // Set to true to enable debug logs
    data: playerMetadata, // Pass metadata object
  },
  shaka, // Shaka Player instance (mandatory field)
);

// Load the video content
const videoUrl =
  "https://stream.fastpix.io/027a90e4-f5e2-433d-81e5-b99ee864c3f6.m3u8"; // Replace with your video manifest URL

player
  .load(videoUrl) // Load the video manifest into the Shaka Player
  .then(() => {
    // Successfully loaded the manifest. FastPix data will begin tracking.
  })
  .catch((error) => {
    fastPixShakaIntegration.handleLoadError(error);
  });

// Ensure to call the following methods to clean up resources when needed:

// player.destroy() - Destroys the Shaka Player instance and releases associated resources
// player.fp.destroy() - Ends FastPix tracking and properly terminates the session
```

### HTML

Include the HTML code below to integrate Shaka Player with FastPix:

```html
<div>
  <video id="video-player" controls></video>
</div>

<script>
  // Wait for the Shaka Player and FastPix Data SDK to be ready
  document.addEventListener("DOMContentLoaded", function () {
    // Initialize player setup
    const initTime = loadShakaPlayer.utilityMethods.now(); // Captures the player initialization time
    const videoElement = document.getElementById("video-player"); // HTML5 video element for Shaka Player
    const player = new shaka.Player(videoElement); // Shaka Player instance bound to the video element

    // Define player metadata
    const playerMetadata = {
      workspace_id: "WORKSPACE_KEY", // Replace with your workspace key
      player_name: "PLAYER_NAME", // A unique identifier for the player instance
      player_init_time: initTime, // The time the player was initialized

      // Additional metadata can be added here
    };

    // Configure FastPix data integration
    const fastPixShakaIntegration = loadShakaPlayer(
      player,
      {
        debug: false, // Set to true to enable debug logs
        data: playerMetadata, // Pass metadata object
      },
      shaka, // The Shaka Player instance (mandatory field)
    );

    // Load the video content
    const videoUrl =
      "https://stream.fastpix.io/027a90e4-f5e2-433d-81e5-b99ee864c3f6.m3u8"; // Replace with your video manifest URL

    player
      .load(videoUrl) // Load the video manifest into the Shaka Player
      .then(() => {
        // Successfully loaded the manifest. FastPix data will begin tracking.
      })
      .catch((error) => {
        fastPixShakaIntegration.handleLoadError(error);
      });

    // Cleanup: Destroy the player instance and stop FastPix data tracking when the page is reloaded or navigated
    window.onbeforeunload = function () {
      player.fp.destroy(); // Ends FastPix tracking
      player.destroy(); // Destroys the Shaka Player instance
    };
  });
</script>
```

Failing to pass the `shaka` object will cause `loadShakaPlayer` to search for it on the global window object. Always ensure to explicitly pass the `shaka` object to avoid any issues.

To end FastPix data tracking, always call `player.fp.destroy()` when calling `player.destroy()`. This ensures proper cleanup of both Shaka Player and FastPix data tracking. 

```javascript
// player is the instance returned by `new shaka.Player` 

player.fp.destroy(); // Ends FastPix tracking 
player.destroy(); // Destroys the Shaka Player instance 
```

# Changing Video Streams

When playing multiple videos back-to-back, notify the FastPix SDK whenever a new video starts to ensure accurate tracking.

### When to Signal a New Source:

- The player advances to the next video in a playlist.
- The user selects a different video to play.

### Emitting a `videoChange` Event:

To inform the FastPix SDK of a new view, emit a `videoChange` event immediately after loading the new video source:

```javascript
// player is the instance returned by `new shaka.Player`
player.fp.dispatch("videochange", {
  video_id: "abc345", // Unique identifier for the new video
  video_title: "My Other Great Video", // Title of the new video
  video_series: "Weekly Great Videos", // Series name if applicable

  // ... and other metadata
});
```

## Advanced Options for FastPix Integration

### 1. Disable Cookies

By default, FastPix uses cookies to track playback across page views. To disable this feature, set `disableCookies: true`.

```javascript
// player is the instance returned by `new shaka.Player`
loadShakaPlayer(
  player,
  {
    debug: false,
    disableCookies: true,
    data: {
      workspace_id: "WORKSPACE_KEY", // Replace with your workspace key

      // ... and other metadata
    },
  },
  shaka,
);
```

### 2. Override “Do Not Track” Behaviour

FastPix does not respect the 'Do Not Track' setting by default. If you want to honor users' privacy preferences, enable this feature by passing `respectDoNotTrack: true`.

```javascript
// player is the instance returned by `new shaka.Player`
loadShakaPlayer(
  player,
  {
    debug: false,
    respectDoNotTrack: true,
    data: {
      workspace_id: "WORKSPACE_KEY", // Replace with your workspace key

      // ... and other metadata
    },
  },
  shaka,
);
```

### 3. Customized Error Tracking Behaviour

Errors tracked by FastPix are considered fatal. If you encounter non-fatal errors that should not be captured, you can emit a custom error event to provide more context.

```javascript
// player is the instance returned by `new shaka.Player`
player.fp.dispatch(
  "error",
  {
    player_error_code: 1008, // Custom error code
    player_error_message: "Description of error", // Generalized error message
    player_error_context: "Additional context for the error", // Instance-specific information
  },
  shaka,
);
```

### 4. Disable Automatic Error Tracking

For complete control over which errors are counted as fatal, consider disabling automatic error tracking by setting `automaticErrorTracking: false`.

```javascript
// player is the instance returned by `new shaka.Player`
loadShakaPlayer(
  player,
  {
    debug: false,
    automaticErrorTracking: false,
    data: {
      workspace_id: "WORKSPACE_KEY", // Replace with your workspace key

      // ... and other metadata
    },
  },
  shaka,
);
```

# Detailed Usage:

For more detailed steps and advanced usage, please refer to the official [FastPix Documentation](https://docs.fastpix.io/docs/monitor-shaka-player).
