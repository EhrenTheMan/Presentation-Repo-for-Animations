# Demo Code Map (Edit Guide)

This guide is built to help you jump straight to the right place when changing behavior, UI, timing, and text.

## Quick Start

1. Open the file for the demo you want.
2. Use the function names and element IDs below as search anchors.
3. Edit only the small section called out.

Primary files:
- [index.html](index.html)
- [Demo1.html](Demo1.html)
- [Demo2.html](Demo2.html)
- [Demo3.html](Demo3.html)

---

## Global Navigation

### Landing Page
File: [index.html](index.html)

What it controls:
- Home page title, description text, and demo buttons.
- Links to each demo.

Where to edit:
- Button labels and destinations in `.nav-links`.
- Styling for homepage cards/buttons in the `<style>` block.

---

## Demo 1: Temperature & Sampling
File: [Demo1.html](Demo1.html)

### Section Map

1. Static data (candidate words and base probabilities)
- Search: `const baseData = [`
- Purpose: core token candidates and base probabilities.
- Typical edits:
  - Add/remove candidate words.
  - Rebalance default probabilities.

2. UI element wiring
- Search these IDs:
  - `aiAssistantBubble`
  - `tempSlider`
  - `tempValue`
  - `optionsList`
  - `generateBtn`
  - `historyList`
- Purpose: binds JS logic to screen elements.

3. Probability math and temperature behavior
- Function: `updateProbabilities()` at [Demo1.html](Demo1.html#L474)
- Function: `weightedRandom(probs)` at [Demo1.html](Demo1.html#L497)
- Typical edits:
  - Change how temperature reshapes distribution.
  - Clamp low-probability words.

4. Arrow/bar animation and selection UX
- Function: `clearArrows()` at [Demo1.html](Demo1.html#L507)
- Function: `showArrow(index)` at [Demo1.html](Demo1.html#L514)
- In click handler, tweak:
  - `maxJumps`
  - `intervalTime`
- Typical edits:
  - Faster/slower pre-selection animation.
  - Different highlight color.

5. Final selected token output + history
- Function: `finishGeneration()` at [Demo1.html](Demo1.html#L567)
- Typical edits:
  - Change sentence starter (`aiStarterText`).
  - Change max history items (currently `8`).

6. Glossary system
- Data object: `const glossaryEntries = {...}`
- Functions:
  - `populateGlossary()` [Demo1.html](Demo1.html#L613)
  - `toggleGlossaryItem(key)` [Demo1.html](Demo1.html#L632)
  - `openGlossary(keyToScroll)` [Demo1.html](Demo1.html#L641)
  - `closeGlossary()` [Demo1.html](Demo1.html#L654)

### Common Demo1 Tweaks
- Add candidate words: edit `baseData`.
- Make demo feel more random: increase slider default or flatten base probs.
- Make generation snappier: lower `maxJumps` and/or `intervalTime`.

---

## Demo 2: Stop Token
File: [Demo2.html](Demo2.html)

### Section Map

1. Script constants and token script
- Search: `const initialText =`
- Search: `const tokensToGenerate = [`
- Purpose: defines exact generated sequence and where stop token appears.
- Typical edits:
  - Rewrite response content.
  - Move `<|stop|>` earlier/later.

2. Core UI elements
- Key IDs:
  - `aiOutput`
  - `tokenStream`
  - `statusText`
  - `generateBtn`
- Purpose: output bubble, engine stream, status messaging, replay control.

3. Animation lifecycle
- Function: `resetAnimation()` at [Demo2.html](Demo2.html#L448)
- Click handler on `generateBtn` (main token loop via `setInterval(..., 250)`).
- Typical edits:
  - Change token cadence by editing interval delay (`250`).
  - Add pause effects before stop token.

4. Stop-token behavior
- In click loop: branch where `currentToken.type === 'stop'`.
- Purpose: halts generation, removes cursor, updates status, reenables button.
- Typical edits:
  - Different stop message.
  - Replay delay (`setTimeout(..., 1000)`).

5. Glossary system
- Function set:
  - `populateGlossary()` [Demo2.html](Demo2.html#L525)
  - `toggleGlossaryItem(key)` [Demo2.html](Demo2.html#L544)
  - `openGlossary(keyToScroll)` [Demo2.html](Demo2.html#L553)
  - `closeGlossary()` [Demo2.html](Demo2.html#L566)

### Common Demo2 Tweaks
- Change narrative text: edit `initialText` and `tokensToGenerate`.
- Speed up replay: lower the interval delay and completion timeout.
- Highlight stop token harder: tweak `.token.stop-token` CSS.

---

## Demo 3: Full Walkthrough (5 Parts)
File: [Demo3.html](Demo3.html)

This file is organized by stage comments in JS and by sections in HTML:
- `stage-0` through `stage-4`

### Global Layout + Routing

1. Tab/stage navigation
- Function: `renderStageNav()` [Demo3.html](Demo3.html#L500)
- Function: `setStage(i)` [Demo3.html](Demo3.html#L510)
- Data: `const stageTitles = [...]`

2. Procedural moving background
- Search: `const bgFxCanvas = document.getElementById('bgFxCanvas')`
- Function: `initProceduralBackground()`
- Purpose: draws animated gradient orbs on a fixed canvas behind the demo UI.
- Typical edits:
  - Increase/decrease orb count via `orbCount`.
  - Change motion intensity with `xAmp`, `yAmp`, `xSpeed`, `ySpeed`.
  - Change colors via `palette`.
  - Tune performance via `DPR_MAX`.

3. Shared utility math/rendering
- `pseudoTokenize(text)` [Demo3.html](Demo3.html#L516)
- `softmaxTemp(probs, t)` [Demo3.html](Demo3.html#L530)
- `weightedSample(probs)` [Demo3.html](Demo3.html#L535)
- `renderBars(...)` [Demo3.html](Demo3.html#L544)

### Part 1 (stage-0): Tokenization Playground

Main controls:
- IDs:
  - `tokenPresetModeBtn`
  - `tokenCustomModeBtn`
  - `tokenPreset`
  - `tokenizeBtn`
  - `tokenClearBtn`
  - `tokenInput`
  - `tokenPieces`
  - `tokenStatus`
  - `tokenCount`
  - `tokenCharCount`

Main logic:
- `stopTokenReveal()` [Demo3.html](Demo3.html#L568)
- `makeTokenChip(text)` [Demo3.html](Demo3.html#L575)
- `getTokenRevealDuration(tokenTotal)` [Demo3.html](Demo3.html#L582)
- `renderStage1Tokenization(...)` [Demo3.html](Demo3.html#L587)
- `resetStage1TokenOutput(...)` [Demo3.html](Demo3.html#L626)
- `setTokenPromptMode(mode)` [Demo3.html](Demo3.html#L633)
- `refreshStage1PromptPreview()` [Demo3.html](Demo3.html#L645)
- `initPromptSelectors()` [Demo3.html](Demo3.html#L655)

Quick edits:
- Reveal speed curve: change formula in `getTokenRevealDuration`.
- Premade prompts: edit `const prompts = { ... }`.
- Token splitting behavior: edit `pseudoTokenize`.
- Token chip scaling: edit `.token` and `.token-wrap` CSS.

### Part 2 (stage-1): Sampling Demo

Main IDs:
- `sampleBars`, `sampleResult`, `sampleStatus`, `sampleOnceBtn`, `sampleResetBtn`

Main logic:
- `sampleTimeline` data array
- `renderSampleBars(...)` [Demo3.html](Demo3.html#L716)
- `resetSampleStage()` [Demo3.html](Demo3.html#L724)

Quick edits:
- Make model more/less deterministic: adjust `sampleTimeline` probs.
- Change sentence path: edit candidate token arrays.

### Part 3 (stage-2): Temperature Visualizer

Main IDs:
- `tempSlider`, `tempValue`, `tempBeforeBars`, `tempAfterBars`, `tempStatus`

Main logic:
- `renderTempStage()` [Demo3.html](Demo3.html#L758)

Quick edits:
- Slider limits: change input min/max/step in HTML.
- Messaging: edit `tempStatus` text branches.

### Part 4 (stage-3): Stop Token Demo

Main IDs:
- `stopPlayBtn`, `stopResetBtn`, `stopStream`, `stopOutput`, `stopStatus`

Main logic:
- `resetStopStage()` [Demo3.html](Demo3.html#L780)
- `runStopStage()` [Demo3.html](Demo3.html#L789)
- `stopTokens` array controls exact stream.

Quick edits:
- Playback speed: change `setInterval(..., 350)`.
- Different sentence: change `stopStarter` and `stopTokens`.

### Part 5 (stage-4): Combined Engine Loop

Main IDs:
- `comboPrompt`, `comboTemp`, `comboTempValue`, `comboSpeed`, `comboPlayBtn`, `comboStepBtn`, `comboOutput`, `comboContext`, `comboBars`, `comboPhaseStatus`

Main logic:
- `comboPlans` object (all prompt-specific timelines)
- `currentComboPlan()` [Demo3.html](Demo3.html#L892)
- `getComboBaseProbs(...)` [Demo3.html](Demo3.html#L897)
- `renderComboContext()` [Demo3.html](Demo3.html#L907)
- `resetCombo()` [Demo3.html](Demo3.html#L916)
- `renderComboTempValue()` [Demo3.html](Demo3.html#L929)
- `comboStep()` [Demo3.html](Demo3.html#L933)
- `comboPlayToggle()` [Demo3.html](Demo3.html#L967)

Quick edits:
- Different answer styles: edit `comboPlans` timelines.
- Engine speed feel: adjust `Math.max(250, Math.round(900 / speed))`.
- Stop behavior: edit final `<|stop|>` position in each timeline.

### Glossary in Demo3

Functions:
- `populateGlossary()` [Demo3.html](Demo3.html#L994)
- `toggleGlossaryItem(key)` [Demo3.html](Demo3.html#L1006)
- `openGlossary(keyToScroll)` [Demo3.html](Demo3.html#L1011)
- `closeGlossary()` [Demo3.html](Demo3.html#L1023)

Quick edits:
- Add terms by extending `glossaryEntries`.
- Adjust open/close behavior in the click listener near the bottom.

---

## Fast Search Cheatsheet

Use these exact searches in VS Code:
- `// Stage 1`
- `const prompts =`
- `const sampleTimeline =`
- `const comboPlans =`
- `function renderStage1Tokenization`
- `function runStopStage`
- `function comboStep`
- `const tokensToGenerate =`
- `const baseData =`

---

## Safe Editing Workflow

1. Change one section at a time.
2. Reload the page after each change.
3. If behavior breaks, revert only the last small block and retest.
4. Keep data arrays (`prompts`, `sampleTimeline`, `comboPlans`, `tokensToGenerate`) well-formed before editing logic.
