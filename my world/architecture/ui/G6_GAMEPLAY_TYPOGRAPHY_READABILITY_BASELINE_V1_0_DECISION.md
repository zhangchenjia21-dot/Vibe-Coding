---
title: my world｜G6 Gameplay Typography Readability Baseline v1.0
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-08
updated: 2026-09-08
phase: G6 cross-package readability correction
owner: Owner + GPT
---

# G6｜Gameplay Typography Readability Baseline v1.0

## 1. Trigger

Owner real UAT after MW-022 confirmed Debug Mode is useful enough, but found the game-page typography materially too small, especially the right-side information surface.

Owner direction:

> 游戏页面的所有字体大小都可以默认改为目前主聊天对话区域的字体大小。

## 2. Frozen interpretation

The current Narrative body is the reference reading scale.

```text
Narrative body reference = 20px
standard gameplay text/control baseline = >= 20px
```

This is a **minimum/default reading baseline**, not a command to flatten hierarchy.

Therefore:

- body copy, buttons, tabs, labels, status/help/error text, side-panel content, Debug rows, recommendation controls and ordinary input/UI text should not intentionally render below 20px on the active gameplay page;
- existing larger semantic hierarchy such as 28px / 40px titles remains larger;
- larger headings may stay as-is unless a concrete layout issue requires bounded adjustment;
- do not reduce the current Narrative body below 20px.

## 3. Product principle

For this text-first RPG, sustained readability outranks information density.

The product should prefer:

```text
larger readable text + scrolling/wrapping
```

over:

```text
smaller text merely to fit more information at once
```

This is a usability baseline, not optional visual polish.

## 4. Scope

Apply the baseline to the **active gameplay surface**, including current first-party runtime UI such as:

- TopBar ordinary controls/status text;
- Narrative auxiliary labels/status/error/composer controls where below baseline;
- recommendation labels/buttons/helper text;
- World Information navigation and all current information surfaces;
- Character / Important Experiences / People presentation;
- Save/Restore gameplay surface controls;
- Debug Mode rows and related gameplay-only labels;
- other currently visible gameplay labels/buttons with an effective size below the baseline.

Main Narrative content remains the reference.

Main Menu / New Game Wizard are not the primary target of this correction. If they inherit a harmless root-theme baseline change, that is acceptable, but do not broaden into a menu/wizard visual redesign.

## 5. Layout accommodation

Raising fonts may require bounded presentation adjustments.

Allowed:

- modest increases to control minimum height/padding;
- wrapping;
- local scroll-region height/width adjustments;
- preserving readable column widths;
- allowing more vertical scrolling.

Not allowed:

- shrinking text back below 20px to preserve density;
- changing information architecture;
- moving/removing product surfaces merely to make text fit;
- global redesign of colors, navigation, composition or visual identity;
- Dynamic UI implementation;
- arbitrary responsive framework work.

## 6. Acceptance

Engineering must prove at minimum:

1. Main Narrative body remains >=20px.
2. All visible ordinary text/control fonts on the active gameplay surface at tested standard window sizes are >=20px, except intentionally larger hierarchy which remains >=20px.
3. Right-side World Information content is no longer materially smaller than Narrative body.
4. Debug Mode text is >=20px and remains usable with scrolling if fewer rows fit.
5. Recommendation / composer / Save / navigation controls remain usable.
6. 960×540, 1280×720 and 1920×1080 do not introduce unusable overlap, clipped controls or horizontal overflow in the normal core surfaces.
7. Any increased vertical scrolling is acceptable if content remains accessible.
8. No gameplay/domain/persistence/Provider behavior changes.

## 7. Non-scope

Do not include:

- font-family redesign;
- custom font asset work;
- color-theme redesign;
- animation;
- complete layout redesign;
- new responsive breakpoints unless unavoidable for a concrete regression;
- Package 2 OOC/Character-guided recommendation features;
- Open Threads/System/Inventory;
- Dynamic UI;
- Application Shell general refactor.

## 8. Gate

This correction is completed before Package 2 because every later Owner UAT depends on comfortable reading.

Owner acceptance question:

> 游戏页面是不是终于能以主聊天正文同等级的字号舒服地长期阅读，而不是右侧和辅助区域明显偏小？
