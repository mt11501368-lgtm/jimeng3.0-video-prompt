---
name: jimeng-video-prompt
description: Generate, rewrite, diagnose, and optimize Chinese prompts for Jimeng/即梦 Video 3.0 and 3.0 Pro text-to-video, image-to-video, and image-plus-text-to-video workflows. Use when a user asks for 即梦视频提示词, wants to turn an idea or reference image into a controllable video prompt, needs camera/action/style/emotion wording, wants prompt variants, or needs help fixing weak motion, ignored actions, unstable anatomy, poor physical realism, or overly complex prompts.
---

# 即梦视频提示词

Turn a visual idea into a direct, controllable Chinese prompt. Prefer concrete natural language over keyword piles.

## Workflow

1. Identify the mode:
   - Text-to-video (T2V): build the whole scene from text.
   - Image-to-video or image+text-to-video (I2V / I+T2V): treat the image as the source of appearance, composition, and style; write mainly about change over time.
   - First/last-frame generation: describe the transition path and continuity between endpoints.
2. Extract only information that affects the generated video:
   - subject and count
   - action sequence
   - scene
   - shot size, viewpoint, and camera movement
   - visual style
   - emotion/performance
   - lighting and atmosphere
3. Decide motion complexity before writing:
   - Prefer one subject and one clear action for reliability.
   - For multiple actions, order them chronologically and connect them explicitly.
   - For interaction, assign every action to a named subject.
4. Draft a concise base prompt using the mode-specific structure.
5. Add only the controls needed for the user’s goal.
6. Run the quality checks below and simplify if reliability is at risk.
7. Return the final prompt first. Add a short structure breakdown or alternatives only when useful.

## Mode-specific structures

### Text-to-video

Use:

`clear subject + action + scene + camera + style + optional emotion + optional lighting`

Start from:

`[subject] 在 [scene] [action]. [camera]. [style/lighting/emotion].`

Describe appearance when identity, clothing, age, materials, or silhouette matter. A vague visual idea may use a short scene description and let the model explore.

### Image-to-video / image+text-to-video

Use:

`action + camera + optional emotion/lighting change`

Do not redundantly redescribe stable information already visible in the image. State who moves, how they move, direction, speed, amplitude, and what the camera does.

Start from:

`[visible subject] [action with direction/speed/amplitude]. [camera movement]. [emotion or lighting change].`

### First/last-frame transition

State:

`starting state + continuous transition/action path + ending state + camera continuity`

Preserve identity, spatial relationships, lighting logic, and motion direction unless the user requests a transformation or discontinuity.

## Action design

- Write simple action as `subject + verb`: “老虎向前行走”.
- Strengthen action with direction, speed, amplitude, or manner: “蓝色赛车以极快速度加速逼近前方橙色赛车”.
- Use chronological connectors for combined actions: “先…随后…然后…最后…”.
- Keep combined actions to about three major beats by default. More than three often reduces instruction following.
- Avoid simultaneous contradictory actions and pronouns with unclear referents.
- For multiple subjects, label them consistently by stable traits such as “红衣女子” and “黑衣男子”.
- Treat parkour, dance, fighting, complex object interaction, and large-amplitude motion as high risk. Simplify beats, reduce subject count, and generate variants.

## Camera and visual controls

- Keep camera language physically compatible with the action.
- Use one primary camera movement by default: push in, pull out, pan, tilt, follow, orbit, crane, handheld, or locked-off.
- Combine movements only when the sequence is explicit and motivated.
- Specify shot size and viewpoint when composition matters: close-up, medium shot, wide shot, low angle, high angle, overhead, POV.
- Tie camera behavior to the subject: “镜头低机位跟拍赛车，随后快速甩向逼近的蓝车”.
- For speed, combine subject acceleration, tracking camera, foreground parallax, controlled motion blur, and environmental response.
- For beauty, describe observable image properties rather than only “高级感”: color palette, light direction, contrast, texture, depth, lens character, and atmosphere.

Read [references/patterns.md](references/patterns.md) when the request needs detailed templates, diagnosis, or variants.

## Reliability checks

Before finalizing, verify:

- Every action has an explicit subject.
- The action order is unambiguous.
- Direction, speed, and amplitude do not conflict.
- Camera movement does not contradict subject motion.
- T2V contains enough scene and appearance information.
- I2V does not waste tokens repeating the reference image.
- No more than three major action beats are used unless essential.
- Style, emotion, and lighting support rather than overpower the motion.
- The prompt can be read as a short scene, not a bag of keywords.

## Failure recovery

When a result ignores instructions or looks unstable:

1. Rewrite indirect wording as literal observable action.
2. Reduce to one subject and one action.
3. Remove decorative style language until motion works.
4. Split a complex sequence into fewer beats or separate shots.
5. Add direction, speed, amplitude, and contact points.
6. Replace pronouns with stable subject labels.
7. Use a simpler camera move or a locked camera.
8. Offer two or three controlled variants rather than one overloaded prompt.

Do not promise perfect results. Flag high-risk motion and say that multiple generations may be needed.

## Output contract

Default output:

`最终提示词：<one polished Chinese prompt>`

When the user asks for analysis, also include:

- `结构拆解：` mode, subject, action, scene, camera, style, emotion/lighting
- `风险提示：` only genuine generation risks
- `备选版本：` concise / cinematic / motion-first variants as appropriate

Ask a question only when missing information would materially change the mode or result. Otherwise make a reasonable assumption and state it briefly.
