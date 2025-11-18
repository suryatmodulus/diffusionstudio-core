# The video engine for your timeline

[![](https://img.shields.io/badge/Made%20with-TypeScript-blue?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![](https://img.shields.io/discord/1115673443141156924?style=flat&logo=discord&logoColor=white&color=5865F2)](https://discord.com/invite/zPQJrNGuFB)
[![](https://img.shields.io/badge/Follow%20for-Updates-black?logo=x&logoColor=white)](https://x.com/diffusionhq)
[![](https://img.shields.io/badge/Combinator-F24-orange?logo=ycombinator&logoColor=white)](https://www.ycombinator.com/companies/diffusion-studio)

Diffusion Studio Core is a browser based video engine built in TypeScript for fast media composition. Think of it like a game engine that is optimized for video, audio and image workloads. It supports both interactive playback for editing and a high fidelity rendering mode for final output. Developers often use it to build non linear editors or other timeline based media applications (e.g. [Diffusion Studio Pro](https://pro.diffusion.studio)). Under the hood it takes advantage of Canvas2DContext and the WebCodecs API to tap directly into hardware accelerated processing in the browser.


https://github.com/user-attachments/assets/3878f293-ab1b-4bb1-8088-b9493546180a


## Documentation
Visit the [Docs](https://docs.diffusion.studio/docs) for comprehensive guides

## Getting Started
```sh
npm install @diffusionstudio/core
```

Recommended usage:

```typescript
import * as core from "@diffusionstudio/core";

const composition = new core.Composition();
```

## Features
A few highlights: declarative timeline compositions, layering, splitting, shapes, captions, rich text, silence removal, effects, transitions, keyframing, bounding boxes, masking, audio ramps, font management, checkpoints, realtime playback and hardware accelerated rendering.

Let’s look at some of these in action.

### Concatenate two videos
```typescript
const sources = await Promise.all([
  core.Source.from<core.VideoSource>('/intro.webm'),
  core.Source.from<core.VideoSource>('/outro.mp4'),
]);

const layer = await composition.add(
  new core.Layer({
    mode: 'SEQUENTIAL'
  })
);

await layer.add(
  new core.VideoClip(sources[0], {
    range: [2, 8],
  })
);

await layer.add(
  new core.VideoClip(sources[1], {
    range: [2, 12],
  })
);
```

### Apply basic transitions
```typescript
new core.VideoClip(/** source **/, {
  transition: {
    duration: 1,
    type: 'dissolve',
  }
})
```

### Mask an image
```typescript
const mask = new core.RectangleMask({
  width: 640,
  height: 1080,
  radius: 100,
});

new core.ImageClip(/** source **/, { mask });
```

### Animate your clips with key frames
```typescript
new core.TextClip({
  text: "Hello World",
  align: 'center',
  baseline: 'middle',
  position: 'center',
  animations: [
    {
      key: 'rotation',
      frames: [
        { time: 0, value: 0 },
        { time: 2, value: 720 },
      ],
    },
  ]
});
```

### Add basic effects to visual clips
```typescript
new core.RectangleClip({
  position: 'center',
  delay: 6,
  duration: 4,
  effects: [
    {
      type: 'blur',
      value: 10,
    },
    {
      type: 'hue-rotate',
      value: 90
    }
  ]
})
```

## Pricing

You can use the engine for free as long as you keep the "Made with Diffusion Studio" watermark on the rendered video. To remove the watermark, you can purchase a [license key](https://buy.stripe.com/5kQdR94rGczzdGD2dp28800?prefilled_promo_code=EARLYBIRD). It’s 30% off until Nov 30th 2025.

## FAQ

### Do I have to renew the license key?

No. It’s a one time purchase and your key stays valid forever.

### What happens if the license server is down?

There is no license server. Your key is a signed payload created with our private key. The library includes a public key that verifies it locally so it works even without an internet connection.

### What if I lose my key?

We can regenerate it any time. Just email contact |at| diffusion.studio.

### What are the limitations of the key?

You can’t share your key with other organizations. Other than that, feel free to use it across as many of your own apps or domains as you need.

## When to use Diffusion Studio Core
- You’re building a timeline based application such as an NLE that needs to render video in the browser
- Your app needs to compose multiple assets into a video or audio output
- You want a framework agnostic and efficient video engine that works with Svelte, Vue, Solid, Angular and others

## When not to use Diffusion Studio Core
- You need to render videos server side
  - Use Remotion
- You want to compose videos with HTML or React
  - Use Remotion
- You want a framework that already includes frontend components like a timeline or inspector
  - Use Remotion with the [Editor Starter](https://www.remotion.dev/docs/editor-starter)
- You need low level encoding, decoding, muxing, demuxing or transcoding capabilities
  - Use Mediabunny ([Sponsoring is welcome!](https://github.com/Vanilagy/mediabunny?tab=readme-ov-file#sponsoring))
