project_path: /web/fundamentals/_project.yaml
book_path: /web/fundamentals/_book.yaml
description: Get you feet wet with the WebXR Device API.

{# wf_updated_on: 2018-09-13 #}
{# wf_published_on: 2018-11-13 #}
{# wf_blink_components: Blink>WebVR #}

# Try the immersive web {: .page-title }

The immersive web encompasses a spectrum of experiences from complete reality to completely immersive, with various levels of augmented reality in between. To accomplish this, all flavors of the immersive web use the [WebXR Device API](https://immersive-web.github.io/webxr-reference/webxr-device-api/) to control application structure and graphics code to provide content within that structure. This guide will teach you application structure.

![The immersive web spectrum](/web/fundamentals/immersive/images/immersive-spectrum.png)

The immersive web is suitable for a variety of use cases, some of which are shown below.

* Immersive 360° videos
* Traditional 2D (or 3D) videos presented in immersive surroundings
* Data visualizations
* Home shopping
* Art
* Something cool nobody's thought of yet

## Prerequisites

To run the code in this guide, you need to do the following:

* Install [Three.js](https://threejs.org/) on your development machine.
* Install a web server to run on localhost.
* Verify that your Android device is [supported by ARCore](/ar/discover/supported-devices)
* Activate [Developer mode](https://developer.android.com/studio/debug/dev-options) on your Android device.
* Install [ARCore](https://play.google.com/store/apps/details?id=com.google.ar.core&e=-EnableAppDetailsPageRedesign) on your Android device
* Enable [remote debugging](/web/tools/chrome-devtools/remote-debugging/) on your development machine.
* Procure a cable to connect your mobile device to your development machine.
* Enable the WebXR Device API by going to `chrome://flags` on your Android device and enabling `#webxr`.

## Application structure

An immersive application is structured roughly in three parts.

**Get a device:** Includes feature detection and getting a proxy object for an attached device.

**Create a session:** Includes session creation and getting a frame of reference.

**Render content** Covers painting content to the screen.

Rendering is a topic that's itself as complicated as the rest of the application structure. Instructions for rendering will be in a later lesson.

![Immersive web application structure](/web/fundamentals/immersive/images/app-structure.png)

### Get a device

There's not much to say about device enumeration. The XRDevice API is available on navigator so `navigator.xr` is used for feature detection. A call to `xr.requestDevice()` returns a Promise that resolves with the first found device.

```javascript
if (navigator.xr) {
  navigator.xr.requestDevice()
  .then(xrDevice => {
    // Create a session
  });
}
```

Note: To simplify code, the rest of the codes examples will exclude feature detection.

### Create a session

We create sessions in the resolved promise returned by `requestDevice()`. To do that, we call `xrDevice.requestSession()`. Every session is created with an options object. At a minimum, it must have a context from an `[HMTLCanvasElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCanvasElement)`. We're using defaults for the other session option. That gives us a session type called a 'magic window'.

![An example of a magic window session](magic-window.png)

The `requestSession()` method also returns a Promise, inside of which we draw our content.

navigator.xr.requestDevice()
.then(xrDevice => {
  **let htmlCanvasElement = document.createElement('canvas');
  xrPresContext = htmlCanvasElement.getContext('xrpresent');
  let sessionOptions = { outputContext: xrPresContext };
  xrDevice.requestSession(sessionOptions);
  .then(xrSession => {
    // Render content
  });**
});

### Render content

Rendering content is complicated enough to merit its own lesson.
