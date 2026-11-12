# AI video glossary

40 terms used when talking about AI video generation, in the order you usually meet them: model
types, controls, prompt vocabulary, artefacts, and film vocabulary that still applies.

Each entry has a short definition, then a line on why it matters when you are actually rendering
something. Terms are grouped, not alphabetical — read a group, not the file.

**Groups**

1. [Model and task types](#1-model-and-task-types) — terms 1–5
2. [Controls you set before rendering](#2-controls-you-set-before-rendering) — terms 6–11
3. [Prompt vocabulary](#3-prompt-vocabulary) — terms 12–14
4. [Model internals](#4-model-internals) — terms 15–20
5. [Adaptation and reference](#5-adaptation-and-reference) — terms 21–26
6. [Post and repair](#6-post-and-repair) — terms 27–30
7. [Artefacts](#7-artefacts) — terms 31–33
8. [Film vocabulary that still applies](#8-film-vocabulary-that-still-applies) — terms 34–40

---

## 1. Model and task types

### 1. Text-to-video (T2V) — 文生视频

Generating a video from a written description alone, with no source image. The model decides
everything the prompt does not: framing, subject identity, palette, motion.

*In practice:* T2V gives you the most freedom and the least control. Use it for establishing shots
and atmosphere; use an image source when a specific object or face has to stay the same.

### 2. Image-to-video (I2V) — 图生视频

Animating a still image. The first frame is given, and the model generates motion forward from it.

*In practice:* I2V is the cheapest way to keep a product, a character or a location consistent
across shots — render or shoot one still, then animate variations of it.

### 3. Video-to-video (V2V) — 视频转视频

Restyling or transforming existing footage, keeping its motion and timing. The structural
information (where things move) comes from the input clip.

*In practice:* V2V is how you get a specific camera move you can actually control: shoot it on a
phone, then restyle. Timing and framing survive; the look does not.

### 4. First and last frame — 首尾帧

A mode where you supply both the opening and the closing image, and the model generates the
movement between them.

*In practice:* the most controllable motion you can get from a prompt-only workflow. Pick two
compatible frames a second or two apart — not two shots — or the model will invent a dissolve.

### 5. Keyframe — 关键帧

An image that defines a moment the render must pass through. In AI video, keyframes are usually
first/last frame pairs, or the anchors of a longer generated sequence.

*In practice:* keyframes are promises. If the two keyframes disagree about lighting or lens, the
middle will average them and look soft rather than wrong, which is harder to fix.

---

## 2. Controls you set before rendering

### 6. Motion brush — 运动笔刷

A painted region on the image that is told to move, plus a direction and strength. Everything
outside the brush is expected to stay still.

*In practice:* good for one moving element (hair, steam, a curtain). Two brushes on overlapping
regions fight each other and produce a warp.

### 7. Camera control — 镜头控制

Explicit camera parameters: pan, tilt, zoom, orbit, dolly, and their speed. When offered as
sliders, they override whatever the prompt says about the camera.

*In practice:* one axis at a time. A pan plus a dolly in one render gives a drift that reads as a
mistake even when both were intended.

### 8. Temporal consistency — 时序一致性

Whether an object stays recognisably the same object from frame to frame: same shape, same
colour, same number of buttons.

*In practice:* the first thing to fail in long renders and the main reason to cut at 3–5 seconds.
Check the second-to-last frame, not the last.

### 9. Character consistency — 人物一致性

Keeping a face, body and wardrobe stable across shots, not just across frames.

*In practice:* achieved with reference images or a trained identity, not with adjectives. A
description like "woman in her 30s" gives you a different person every render.

### 10. Lip sync — 口型同步

Matching mouth shapes to an audio track. Either generated from the audio, or driven by a driving
video.

*In practice:* always check on a close-up at normal speed. Sync that looks fine paused can be a
frame off, which reads as a badly dubbed film.

### 11. Frame rate (fps) — 帧率

Frames per second of the output. 24 for a film look, 25/30 for broadcast and social, 60 for
smooth motion, 12 for stylised.

*In practice:* rendering at 24 and playing back at 30 speeds the motion up by 25%; the same clip
suddenly looks like a timelapse. Set it before you render, not after.

---

## 3. Prompt vocabulary

### 12. Prompt adherence — 提示词还原度

How literally the model follows the prompt: exact colours, exact counts, exact wording.

*In practice:* the first clause wins. Put the thing that must be right in the first ten words;
that is the part models weight most and the part they drop least.

### 13. Negative prompt — 负向提示词

Text describing what should not appear. Supported by most T2V/I2V models, weighted separately.

*In practice:* use it for faults you have seen more than once. A negative list longer than about
15 tokens starts silently dropping terms.

### 14. Seed — 随机种子

The number that fixes the random starting state. Same seed, same prompt, same model version →
the same video.

*In practice:* write the seed next to every render you keep. Without it, a good result is not
reproducible and cannot be extended.

---

## 4. Model internals

### 15. Inference steps — 采样步数

How many refinement passes the model makes from noise to image. More steps, more detail, more
time, with diminishing returns.

*In practice:* most models are near their best around 25–40 steps. Going to 100 mostly buys you
render time and a slightly harder look.

### 16. Guidance scale (CFG) — 引导强度

How strongly the render is pushed toward the prompt versus letting the model do what it likes.

*In practice:* too low drifts off-prompt; too high oversaturates and freezes motion. On video,
lower guidance usually means more natural movement.

### 17. Latent space — 潜空间

The compressed representation the model actually works in. Video is generated as latents and
decoded to pixels at the end.

*In practice:* this is why small prompt changes can produce large motion changes — you are editing
a compressed description, not a scene.

### 18. Diffusion model — 扩散模型

A model trained to remove noise step by step, starting from random noise and ending at a coherent
image or clip.

*In practice:* explains the artefacts you see: flicker is inconsistent denoising between frames,
not a lighting error.

### 19. DiT — 扩散 Transformer

Diffusion with a transformer backbone instead of a U-Net. Most current video models are DiTs;
the architecture is why they scale to longer clips.

*In practice:* DiT models tend to hold composition over longer durations, and to be more
sensitive to prompt structure than older U-Net models.

### 20. VAE — 变分自编码器

The encoder/decoder pair that compresses pixels to latents and back.

*In practice:* a weak VAE shows up as softness and colour banding in flat areas — sky gradients
are the usual tell.

---

## 5. Adaptation and reference

### 21. LoRA — 低秩适配

A small set of weights that modifies a base model's behaviour: a style, a character, a product.

*In practice:* LoRAs are how consistency is usually solved at scale. A LoRA trained on one product
beats any amount of prompt wording about that product.

### 22. Fine-tuning — 微调

Further training of a model on your own data. Broader than a LoRA and more expensive.

*In practice:* rarely worth it for one video; usually worth it when a look has to be reproduced
across dozens of shots.

### 23. Reference image — 参考图

An image that tells the model what something should look like, without being the first frame.

*In practice:* reference images control identity and material; first frames control composition.
Using one image for both is why some renders start perfectly and then drift.

### 24. Inpainting — 局部重绘

Regenerating a masked region of an existing frame or clip while keeping the rest.

*In practice:* the cheapest fix for a single bad hand or a wrong logo. Mask generously — a tight
mask leaves a visible seam.

### 25. Outpainting — 画面扩展

Extending the frame beyond its original borders, generating new content around the existing image.

*In practice:* how a 16:9 render becomes 9:16 without cropping the subject. Extend in small steps;
one large expansion invents a new room.

### 26. Upscaling — 放大

Increasing resolution after generation, usually with a separate model.

*In practice:* upscale last, after you have locked the cut. Upscaling an unapproved shot wastes
the most expensive step in the pipeline.

---

## 6. Post and repair

### 27. Frame interpolation — 补帧

Generating in-between frames to raise the frame rate (24 → 60, for example).

*In practice:* it smooths motion but also smooths artefacts into slow, visible warps. Interpolate
the final cut, never the source clips you still plan to re-render.

### 28. Flicker — 闪烁

Per-frame brightness or colour instability; the image "breathes" without anything in the scene
changing.

*In practice:* mostly an exposure-estimation problem. Fix with a negative prompt entry, a locked
seed, and a brightness-stabilisation pass — in that order.

### 29. Warping — 形变

Geometry that bends where it should be rigid: straight lines curving, frames of a door shifting.

*In practice:* worst on long lenses and shallow focus. If a shot has to hold an architectural
line, shoot it wide and crop.

### 30. Ghosting — 重影

A translucent duplicate of a subject left behind, usually where motion is fast or the model is
uncertain about identity.

*In practice:* a symptom of too much motion for the duration. Shorten the action or extend the
shot; do not try to prompt it away.

---

## 7. Artefacts

### 31. Motion blur — 运动模糊

Directional blur from movement during the exposure. Real cameras produce it; models approximate
it.

*In practice:* consistent blur sells speed. If one shot in a sequence has crisp edges and the next
is smeared, the cut reads as a different camera even when the framing matches.

### 32. Shutter angle — 快门角度

The fraction of each frame interval the shutter is open: 180° is the film default, giving natural
motion blur.

*In practice:* a prompt that says "crisp, no blur" is asking for a 0° shutter, which looks like a
surveillance camera. Name the shutter angle if the model exposes it.

### 33. Depth of field — 景深

The range of distances that stay sharp. Shallow depth of field isolates a subject; deep keeps a
scene readable.

*In practice:* shallow is the default look of AI video and the reason bad background geometry
survives. Deep focus exposes it.

---

## 8. Film vocabulary that still applies

### 34. Parallax — 视差

The different apparent speed of near and far objects when the camera moves. It is what makes a
move feel three-dimensional.

*In practice:* ask for a lateral move with foreground objects. A push-in on a flat background has
no parallax, which is why some generated camera moves feel like a zoom.

### 35. Shot size — 景别

How much of the subject fills the frame: ECU, CU, MCU, MS, WS.

*In practice:* decide shot size before writing the prompt. Most "the model ignored me" complaints
are a prompt that never stated the size.

### 36. Match cut — 匹配剪辑

Cutting between two shots that share a shape, movement or composition, so the cut is felt rather
than noticed.

*In practice:* the cleanest way to join two generated clips that do not share a location. Match the
shape, not the subject.

### 37. Whip pan — 甩镜

A fast pan that blurs the frame and lands on a new subject. Often used to hide a cut.

*In practice:* whip pans are forgiving on generated footage because the blur covers the transition —
one of the few moves where an imperfect render still works.

### 38. Speed ramp — 变速

Changing playback speed within a shot, usually slowing at the moment of impact.

*In practice:* render at a high frame rate and ramp in post. Asking a model to "ramp" mid-generation
usually produces a stutter instead of a smooth slowdown.

### 39. Aspect ratio — 画幅比

The frame's width-to-height ratio: 16:9 landscape, 9:16 vertical, 1:1 square, 2.39:1 scope.

*In practice:* decide it before rendering. Cropping 16:9 to 9:16 loses the composition; outpainting
to 9:16 keeps it but invents the edges.

### 40. Provenance metadata — 来源元数据

Data attached to a file recording how it was made — model, prompt, seed, and often a signed
marker such as C2PA.

*In practice:* the useful half is the seed and the model version, which make a render reproducible.
Keep them with the file, not in a chat log.
