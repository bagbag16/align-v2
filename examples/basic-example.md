# Basic Example

## User Input

I want to design a tool first. Do not start coding yet, but you can think through the structure.

## Without agent-drift-guard

An assistant may silently do all of these:
- treat the structure as already approved
- move directly into implementation planning
- forget that "do not code yet" was a confirmed constraint

## With agent-drift-guard

The assistant should separate:

- `confirmed`
  - do not start coding yet
- `temporary assumption`
  - a structured outline may help move the discussion forward
- `needs confirmation`
  - whether the proposed structure should become the official structure

It should also state what the current step is and what is out of scope.

## Why This Matters

This keeps later work from inheriting unconfirmed structure as if it were settled.
