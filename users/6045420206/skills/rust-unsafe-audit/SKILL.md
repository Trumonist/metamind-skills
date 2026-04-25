---
slug: rust-unsafe-audit
name: Rust Unsafe Auditor
description: Audits Rust unsafe blocks across 5 dimensions: SAFETY comments, aliasing/lifetimes, data races, uninitialized memory UB, and safe alternatives.
triggers:
  - user pastes a Rust unsafe block
  - user asks to audit unsafe Rust code
  - user shows unsafe {} in Rust
---

# SKILL: rust-unsafe-audit

When triggered, audit the provided unsafe Rust code across exactly 5 dimensions. Output **5 numbered findings**, each with a verdict tag and a short comment.

## Verdict levels
- OK -- no issue found
- WARNING -- potential problem, context-dependent
- CRITICAL -- definite or near-certain UB / soundness hole

## The 5 audit points

1. **SAFETY comment** -- Is there a `// SAFETY:` comment immediately above the `unsafe` block explaining which invariants make this sound? Does it cover all unsafe operations in the block?

2. **Aliasing & lifetimes** -- Are there simultaneous `&` and `&mut` references to the same memory? Are raw pointers derived from references that may have been invalidated? Check `as *mut` casts from shared refs, out-of-bounds pointer arithmetic, dangling pointers.

3. **Data race / Send+Sync** -- Could this code be called from multiple threads? Are types crossing thread boundaries properly `Send`/`Sync`? Look for UnsafeCell misuse, raw pointer sharing across threads without synchronization.

4. **Uninitialized memory UB** -- Is `MaybeUninit` used correctly (.assume_init() only after full init)? Is `mem::zeroed()` called on types with non-zero validity requirements (references, NonNull, NonZero*, bool, enums)? Is deprecated `mem::uninitialized()` present?

5. **Safe alternative** -- Can this unsafe block be replaced with safe Rust or a well-audited crate (bytemuck, zerocopy, copy_from_slice, slice::from_raw_parts -> &[u8] via safe wrappers, etc.)? If yes, provide a brief sketch.

## Output format

```
**Rust Unsafe Audit**

1. SAFETY comment       — <verdict> <comment>
2. Aliasing & lifetimes — <verdict> <comment>
3. Data race / Send+Sync — <verdict> <comment>
4. Uninitialized memory — <verdict> <comment>
5. Safe alternative     — <verdict> <comment>

**Summary:** <one-line overall verdict>
```

Verdicts: OK, WARNING, CRITICAL.
If multiple unsafe blocks are pasted, audit each separately under its own header.
Keep each comment to 1-2 sentences. Mark WARNING (needs context) when insufficient info.
