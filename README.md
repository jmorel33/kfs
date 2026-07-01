# Kaizen Filing System (KFS)

**Version 2.3.0** · MIT License · (c) 2025–2026 Jacques Morel

KFS is a **domain-isolated, ACL-secured content store** built on SQLite. It persists actors, domains, security schemes, and content entities (artifacts, topics, epics, notes) as a standalone static library suitable for embedding in a single process — no server required.

This package ships as a header-orchestrated C library (`kfs/kfs.h`) with a unified memory layer (KFS heap + SQLite heap on one backend) and an optional MyBuddy allocator profile (`libkfs_mybuddy.a`) for higher read throughput.

---

## Quick start

```bat
cd sit\kfs
build_kfs.bat    REM → build/libkfs.a
test_kfs.bat     REM → 55/55 harness (H0–H7)
```

Include KFS from your project (add `-I sit/kfs`):

```c
#define KFS_IMPLEMENTATION
#include "kfs/kfs.h"
```

Call `kfs_mem_init()` before any KFS or `sqlite3_*` use if SQLite is shared outside `kfs_init`. See [doc/COMPILATION_GUIDE.md](doc/COMPILATION_GUIDE.md) for link flags, make targets, and the optional MyBuddy build (`libkfs_mybuddy.a`).

---

## Documentation

Full documentation lives in **[doc/](doc/)**. Start with the index:

| Document | What it covers |
|----------|----------------|
| [**doc/README.md**](doc/README.md) | Documentation index |
| [**doc/kfs_guide.md**](doc/kfs_guide.md) | Security model — actors, domains, permissions |
| [**doc/architecture.md**](doc/architecture.md) | Internal design — databases, code layout, memory |
| [**doc/COMPILATION_GUIDE.md**](doc/COMPILATION_GUIDE.md) | Build, link, include paths, make targets |
| [**doc/test_harness_plan.md**](doc/test_harness_plan.md) | Test harness — H0–H7 (55 tests) |
| [**doc/memory_alloc_plan.md**](doc/memory_alloc_plan.md) | Unified memory — production sign-off, MyBuddy perf study |
| [**doc/clone_plan.md**](doc/clone_plan.md) | Clone & lineage (planned) |

Completed plans and benchmark logs: [doc/done/](doc/done/).

Test harness details: [tests/README.md](tests/README.md).

---

## Package layout

```
sit/kfs/
├── kfs/           # Library source (kfs.h, kfs_api.h, kfs_mem.h, impl headers)
├── build/         # Artifacts (libkfs.a, harness binary) — generated
├── tests/         # 55-test harness and fixtures
├── scripts/       # sync_api.py, check_kfs_mem.py (build gate)
├── doc/           # Architecture, guides, plans
├── build_kfs.bat  # Windows wrapper
└── test_kfs.bat
```

Public API declarations: `kfs/kfs_api.h`. Version macros: `kfs/kfs_version.h`.

---

## License

MIT — see [LICENSE](LICENSE).