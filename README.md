<!--
  GitHub 用户主页 README（基于 handle tlangroup）。
  主线：t 语言拥有三种「时间」——comptime / runtime / extendime。
  语言：英文（GitHub 公开项目惯例）。需要中文 / 中英双语版告诉我即可。
  部署：内容放进特殊仓库 <tlangroup>/<tlangroup> 的 README.md 即可渲染到个人主页。
-->

# Hi, I'm building **t** — a language with three times 👋

> **comptime · runtime · extendime**
>
> Every language gives you two phases: code that runs *while compiling* (**comptime**) and code that
> runs *in the program* (**runtime**). `t` adds a third — **extendime**: the moment a project reshapes
> the *compiler itself*, gaining new syntax and semantics for its own domain — **without forking or
> mutating the root compiler**.

[![Self-hosting](https://img.shields.io/badge/self--hosting-%E2%9C%93-brightgreen)](https://github.com/tlangroup/t)
[![Backends](https://img.shields.io/badge/backends-C11%20%7C%20LLVM%20IR-blue)](https://github.com/tlangroup/t)
[![extendime](https://img.shields.io/badge/extendime-%E2%9C%93-brightgreen)](https://github.com/tlangroup/tote)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/tlangroup/t/blob/main/LICENSE)

This account is dedicated to the **t** programming-language ecosystem. Two repositories.

---

## 🕐 The three times of `t`

| time | when it happens | what you get to do |
|------|----------------|--------------------|
| **comptime** | during compilation | run code, fold constants, generate types via `comptime { }` |
| **runtime** | in the running program | the actual executable behavior |
| **extendime** | at build-config time | derive *your own `tc` variant* — extend the language itself |

The first two are familiar. **extendime** is `t`'s distinguishing idea:

- The language is **not a fixed black box**. A project can *extend* the compiler for its own needs —
  new domain syntax, lint rules, codegen tweaks — and ship that *grown* language alongside its code.
- Extensions are **derived, never modified**. Your variant is built *on top of* the root `tc`; the root
  compiler stays pure and untouched.
- Because of that, **hermeticity is structural**: dependencies are always compiled by the *root* `tc`,
  never by your variant. Whole classes of "global feature leaks into dependencies" problems become
  **impossible by construction**, not just discouraged by convention.

In short: **extendime lifts the expressive ceiling of the language** above "whatever its authors shipped."
A project can grow `t` to fit its domain — and still interoperate cleanly with everything else.

---

## 🔹 [`t`](https://github.com/tlangroup/t) — the language & compiler

`t` is a statically-typed, **data-driven / ECS-oriented / AI-Agent-friendly** systems language.
Source files use the `.tc` extension and compile to **C11** or **LLVM IR**.

- 🧠 **Self-hosting** — the compiler (`tc`) is written in `t` itself and bootstraps to a verified
  fixed point (`g2 == g3 == g4` byte-identical), gated green by `tools/gate.mjs` (L2).
- ⚙️ **Dual backend** — emit **C11** (portable, drop-in) or **LLVM IR** (optimized, native).
- 🪄 **comptime metaprogramming** — `comptime { }` blocks run real `t` code during compilation;
  `@compile_log` / `@compile_warning` / `@compile_error` drive compile-time introspection.
- 🤖 **AI-Agent friendly** — `tc check/lex --json` emit machine-readable diagnostics; the conformance
  suite is driven by `// EXPECT:` headers.
- 📦 **Reproducible toolchain** — precompiled `t_runtime.lib` (arena / TStr / TSlice / TMap) with
  automatic source fallback.

A taste of `t`:

```t
@import("std");

fn main() i32 @main {
    println("hello from t");
    return 0;
}
```

Build & run:

```bash
out/dbg/tc.exe run   app.tc          # .tc -> LLVM IR -> exe (needs clang)
out/dbg/tc.exe build app.tc --emit-c # or compile to C11
out/dbg/tc.exe check app.tc --json   # type-check, JSON diagnostics
```

---

## 🔹 [`tote`](https://github.com/tlangroup/tote) — where **extendime** becomes real

`tote` is `t`'s **cargo-shaped** build/package manager — and it is the layer that *operationalizes*
extendime. The `[compiler].extend` directive in `tote.toml` is exactly the **extendime** declaration:
it tells the build to derive this project's `tc` variant, cache it content-addressed, and wire it into
compilation — all without ever touching the root compiler.

- 📜 **Convention over configuration** — `src/`, `tests/`, `examples/`, `benches/` are auto-discovered;
  `tote.toml` is the single source of truth, `tote.lock` is always committed.
- 🔒 **Reproducible builds** — lockfile + content-addressed cache + deterministic resolution.
- 🌿 **Dependencies as first-class** — `path` / `git` / `registry` sources, `normal` / `dev` profiles.
- ✨ **extendime, hermetic** — `extend` derives a *variant*, not a fork. The variant compiles *this*
  project; every dependency is still built by the pristine root `tc`. The language grows locally,
  the ecosystem stays stable.

```bash
out/dbg/tote.exe init    # scaffold tote.toml + src/main.tc
out/dbg/tote.exe build   # build the entry — extendime derives tc', then compiles via it
out/dbg/tote.exe run     # build & run
```

---

## ✨ Why `t`

Most languages treat the compiler as a fixed black box: you live inside the comptime/runtime the
authors gave you, and if the language doesn't fit your domain, you wait for a proposal.

`t` treats the compiler as a **derivable build input**. With **extendime**, a project can reshape the
language for its own needs — and because that reshape is *derived, not mutated*, it stays fully isolated
from the ecosystem. Combined with a clean, data-oriented type system and dual-backend output, `t` aims
to be a practical systems language that is pleasant for both humans **and** agents to drive — and one
whose expressive range is bounded only by your project, not by its maintainers.

---

## 📫 Connect

- 🐛 Issues & PRs: open them in the relevant repo ([`t`](https://github.com/tlangroup/t) · [`tote`](https://github.com/tlangroup/tote)).
- ✉️ Contact: ltg1710@gmail.com

---

<sub>Licensed under MIT. The `t` compiler and `tote` are written in `t` itself — including the `tc` that builds them.</sub>
