<h1 align="center">{{REPO_NAME}}</h1>

<p align="center">
  <em>{{DESCRIPTION}}</em>
</p>

<p align="center">
  <a href="https://github.com/JenkinsRobotics/{{REPO_NAME}}/releases"><img src="https://img.shields.io/badge/version-0.1.0-2EA44F?style=for-the-badge" alt="Version"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-2EA44F?style=for-the-badge" alt="License"></a>
  <img src="https://img.shields.io/badge/python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.11+">
  <img src="https://img.shields.io/badge/JaegerOS-pinned-58A6FF?style=for-the-badge" alt="JaegerOS pinned">
</p>

---

> ## Replace-these checklist (delete this block once done)
>
> - [ ] `{{REPO_NAME}}` — every occurrence, this file + `gh-pages/index.html` + `pyproject.toml.example`
> - [ ] `{{DESCRIPTION}}` — one-line description, this file + `gh-pages/index.html`
> - [ ] `{{package_name}}` — the Python import name (snake_case) — rename the
>       `src/{{package_name}}/` directory itself, and every path inside it
> - [ ] Fill in `src/{{package_name}}/module.yaml` (drop the `.example`
>       suffix from every `*.example` file once real values are in)
> - [ ] Pin a real JaegerOS version range in `pyproject.toml.example`
>       (see [Ecosystem links](#ecosystem-links))
> - [ ] Seed `dev/docs/reality/STATUS.md` with today's date and truth
> - [ ] Enable GitHub Pages from `gh-pages/` (see `gh-pages/SITE.md`)
> - [ ] Pick a LICENSE (Apache-2.0 shipped by default — swap if this repo
>       needs different terms)

---

## What it is

`{{REPO_NAME}}` is a **{{DESCRIPTION}}** built on
[JaegerOS](https://github.com/JenkinsRobotics/JaegerOS) — the framework
layer of the Jaeger ecosystem (Bus · Node · modules/slots · supervisor ·
safety · contract · capability layer). This repo doesn't fork or edit
JaegerOS; it **pins** a release and builds on top of it, the same way a
ROS package builds on ROS.

Depending on what you're building here, this repo is one of:

- **An engine module** — a self-contained capability (`module.yaml` +
  `config.py` + `node.py` + tests) that plugs into any JaegerOS project
  through a **slot** (see [`jaeger_os/nodes/kokoro_tts/`](https://github.com/JenkinsRobotics/JaegerOS/tree/master/jaeger_os/nodes/kokoro_tts)
  for the canonical shape this template mirrors).
- **A hardware package** — a body's capability surface (motors, lights,
  vision, …), registered through the capability layer.
- **A project** — an assembled thing (a robot, an app, a desktop
  companion) that pulls in JaegerOS + whichever modules it needs and owns
  its own bringup (topology, config, instance).

See [`CONVENTIONS.md`](CONVENTIONS.md) for the rules every repo in the
ecosystem follows, and the tier map in
[Ecosystem links](#ecosystem-links).

## Install

```bash
git clone https://github.com/JenkinsRobotics/{{REPO_NAME}}.git
cd {{REPO_NAME}}
python3 -m venv .venv && source .venv/bin/activate
cp pyproject.toml.example pyproject.toml   # after filling in the pin — see checklist above
pip install -e .
```

This repo depends on a **pinned** `jaeger-os` release — it does not
vendor or fork the framework. See `pyproject.toml.example` for where the
version range is declared.

## Quick start

```bash
# Run this module's contract smoke test — proves module.yaml parses,
# the factory builds a live node, and the bus contract round-trips —
# all without touching real hardware or models.
pytest src/{{package_name}}/tests

# Or run it directly:
python -m {{package_name}}.tests.test_module_contract
```

Wire the module into a running JaegerOS instance by pointing its module
discovery at this package (see JaegerOS's `jaeger_os/core/modules.py` for
how `discover_modules()` walks module roots) — or, for a project-tier
repo, declare it in your instance's manifest.

## Module layout

The canonical shape every module/hardware-package repo in the ecosystem
follows, mirroring [`jaeger_os/nodes/kokoro_tts/`](https://github.com/JenkinsRobotics/JaegerOS/tree/master/jaeger_os/nodes/kokoro_tts)
in JaegerOS itself:

```
src/{{package_name}}/
├── __init__.py            ← exports the factory (module.yaml's `factory:` target)
├── module.yaml.example    ← the manifest: slot, topics, tools, requires_*
├── config.py.example      ← this module's settings-catalog schema slice
├── node.py.example        ← the four-phase Node (setup → tick → teardown → health)
├── docs/                  ← this module's own design notes
│   └── DESIGN.md.example
└── tests/
    └── test_module_contract.py.example   ← the module-contract smoke test
```

One copy of every truth: the manifest (`module.yaml`) is the single
source for what this module consumes, produces, and requires — nothing
else in the repo should restate it.

## Development

```bash
pytest src/{{package_name}}/tests    # module-contract smoke test
ruff check src/                      # lint, if configured
```

Follow [`CONVENTIONS.md`](CONVENTIONS.md) — especially: no doc describes
behavior the code doesn't implement yet (mark it `(planned)` instead),
and `dev/docs/reality/STATUS.md` stays truthful — any commit that changes
behavior updates it in the same commit.

CI (`.github/workflows/ci.yml`) runs the test suite and, if this repo
sits below the Mind in the dependency graph, the nervous-system dependency
check (see `CONVENTIONS.md` → "The connection rule").

## Ecosystem links

- [JaegerOS](https://github.com/JenkinsRobotics/JaegerOS) — the framework
  this repo pins (Bus · Node · modules/slots · supervisor · safety ·
  contract · capability layer). You build on it; you don't edit it.
- [Jaeger-AI](https://github.com/JenkinsRobotics/Jaeger-AI) — the turnkey
  agentic product (the Mind): loop, tools, skills, memory, persona, local
  inference, and its own faces (chat app, TUI, voice).
- [JP01](https://github.com/JenkinsRobotics/JP01) — the reference
  hardware Jaeger (the Body); the first repo to consume out-of-tree
  modules and hardware packages the way this template scaffolds.

---

## License

[Apache-2.0](LICENSE) © Jenkins Robotics
