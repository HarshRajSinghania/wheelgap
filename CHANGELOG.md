## Unreleased

- `--ignore NAME` (repeatable, comma-separated) excludes known gaps from the report, summary and exit code, matched case-insensitively. Text, Markdown and JSON output list them as ignored. (#13)

## 0.6.3 (2026-11-09)

`--dockerfile` now reads Dockerfiles the way Docker does, down to whitespace, line endings, tags and stage names, so it errors where Docker would instead of guessing. The coverage scanner adds Python 3.15 and 3.15t, and `--list-targets` shows the glibc 2.17 manylinux floor outside x86.

- The coverage scanner checks Python 3.15 and free-threaded 3.15t by default, alongside 3.13, 3.14 and 3.14t. (#117, thanks [@moelomda](https://github.com/moelomda))
- `--dockerfile` accepts quoted `--platform` values such as `FROM --platform="linux/arm64" python:3.13` (and rejects an unterminated quote). (#101, thanks [@Mr45401](https://github.com/Mr45401))
- `--list-targets` shows the glibc 2.17 floor for non-x86 Linux targets (e.g. `glibc 2.17 to 2.36` for `linux-aarch64`), and the README targets table says so too. A `Pipfile.lock` or `pyodide-lock.json` that isn't a JSON object, or a Pipfile.lock entry with a non-string version, is now a clear error instead of an `AttributeError`. With `-q`, the lock note added in 0.6.2 only appears when some packages couldn't be re-checked. (#99)

### Fixed

- `--dockerfile` rejects an upper-case repository name (`FROM PYTHON:3.13`) or tag variant (`python:3.13-SLIM`) instead of lowercasing it, and a tag with characters Docker doesn't allow, as Docker does. Python versions are matched with ASCII digits only, so `FROM python:3.1٣` and `-p 3.1٣` are errors instead of being read as 3.13. (#108, thanks [@bfs2021](https://github.com/bfs2021))
- `--dockerfile` splits lines on `\n` only, matching Docker: a lone `\r` (old Mac line endings), NEL and other Unicode line breaks no longer start a new line. CRLF files still work. (#106, thanks [@chiragjangiir](https://github.com/chiragjangiir))
- `--dockerfile` splits `FROM` lines on ASCII whitespace only, matching Docker, so a non-breaking space (e.g. after `FROM` or between words) is an error instead of quietly splitting words. A Dockerfile that isn't UTF-8 now gets a clear error. (#103, thanks [@chiragjangiir](https://github.com/chiragjangiir))
- `--dockerfile` rejects stage names Docker rejects (e.g. `AS 1build` or `AS my@stage`) instead of accepting them. (#100, thanks [@chiragjangiir](https://github.com/chiragjangiir))
