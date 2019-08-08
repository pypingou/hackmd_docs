---
title: Gating rawhide packages - Flock 2019
tags: Talk, Fedora, Infra
description: View the slide with "Slide Mode".
---

## Gating rawhide packages
#### things just got real! :tada: 

<br />
<br />
<br />

slide: https://hackmd.io/@pingou/rkCVBOlmS#/
License: CC-BY

---

## The Agenda

- Introduction
- Why gating rawhide?
- The challenge
- State of the art
- Roadmap

---

## Who are we?

<div style="width: 45%; float: right;">

### pingou

- Fedora infrastructure
- Leading rawhide packages gating

</div>

<div style="width: 45%; float: left;">

### Dominik Perpeet

- Fedora CI objective lead
- Manager for Fedora CI

</div>

---

## Why gating rawhide?

---

### Why gating rawhide?

- More stable rawhide
- Working composes
-- Quicker turn-around for beta-releases
- Faster update release
<br />
- _you break it, you fix it_

---

## The challenges

---

### The challenges

- Make it happen
 
- Fit in in the packager's workflow

- Interrupt as little as possible the packager's workflow

- False negatives

---

## Plan

- Phased roll out
-- Single build update gating
-- Multi build updates gating

- Early user feedback

- Polished UX

---

## State of things

- Single build update gated in rawhide
-- How does it work?

---

- Single build updates gating

<div >
<img style="width: 80%;" src="https://pingou.fedorapeople.org/Rawhide%20Package%20Gating%20Flow_v2.jpg">
</div>

---

- Single build updates gating (simplified)

<div >
<img style="width: 90%;" src="https://pingou.fedorapeople.org/Simplified_single_build_update.jpg">
</div>

---


### Early feedback

- It works! :fireworks: :yum: 

- 1 update -> 5 to 7 emails :cry: 

- Do ++**not**++ use buildroot overrides :smiley:

- Signing mass-rebuilds needs adjustments
-- Dedicated signing?
-- More workers?

---

## Roadmap

- Single build updates:
-- Less emails (2 to 3) :joy:
-- Fix for buildroot overrides :satisfied: 

- Multi-build updates

---

### Multi-build updates

---

- Multi build updates gating

<div >
<img style="width: 50%;" src="https://pingou.fedorapeople.org/Multi-builds%20-%20Rawhide%20Package%20Gating%20Flow.png">
</div>

---

- Multi builds updates gating (simplified)

<div >
<img style="width: 90%;" src="https://pingou.fedorapeople.org/Simplified_multi_builds_update.jpg">
</div>

---

### Points of attention with gating

- Side-tags proliferation?

- Merging side-tags
-- Conflicting builds
-- Race condition between bodhi and koji

- Testing side-tags
-- How to test a side-tag?
-- How to report test results about a side-tag?

---

### Points of attention with testing

- Stability
- Impact
- Multi-arch
- Maintainability
- Connecting upstream

---

## Help us

- Test the current workflow
-- Volunteers welcome to test the multi-build updates workflow when it lands
- Give your feedback

---

## Thank you!

### Questions?

Contact us:

- #fedora-ci on freenode
- ci@lists.fedoraproject.org
- https://pagure.io/fedora-ci/general/
