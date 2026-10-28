# mv-playbook

The **entry point** to the accumulated MV skill family — a map plus the cross-cutting hard rules.

Every rule in here cost something: a re-render, a rejected deliverable, or a metric that answered the wrong question. They are written as *symptom → root cause → fix → measured number*, because a lesson without a number is not a lesson.

## What is in it

- **Index** — which skill governs which part of the work (pipeline / arrangement / subtitles / audit / character consistency / image forensics / dispatch / handover).
- **38 hard rules** — e.g. a parameter tweak is invisible on 8-bit footage; a metric can be poisoned by grain; a single-image absolute cannot answer a two-image relative question (three sharpness metrics all pointed the wrong way on the same batch); per-phrase subtitle reveals invite sync-auditing; a lyric timeline file is usually linearly interpolated and off by seconds; a process started by an agent does not survive the turn.
- **Accumulation protocol** — how a new lesson gets filed: which skill, written as symptom→cause→fix→number, and failures go into a "don't try this again" list.

## Why a separate skill

Skills drift into duplication. Without one file that owns the index and the cross-cutting rules, the same lesson gets rewritten in three places and contradicts itself. This is that file.

Rule of thumb: **details go into the specific skill; only cross-cutting rules belong here.**

## Licence

MIT. See `LICENSE`.
