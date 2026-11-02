# Agent instructions

The modern app is PostgreSQL. The legacy source is a separate read-only file.

## Legacy database

`miscelanious/BUY.GDB` is InterBase 7.1, ODS 11.1, page size 4096, dialect 1, Windows-1251. Firebird clients and servers cannot open it. Do not download `fbclient.dll`, do not run `inspect_firebird.py` against this file, and do not install InterBase to browse it.

Do not run `FUNSTART.BAT`, `gfix -mend`, `gfix -sweep`, or `gbak -R`. Do not write to the original file. Work only with `migration/legacy_work/BUY_COPY.GDB`, which is gitignored.

Start the project service:

```bash
mkdir -p migration/legacy_work
cp miscelanious/BUY.GDB migration/legacy_work/BUY_COPY.GDB
chmod a-w migration/legacy_work/BUY_COPY.GDB
make legacy
```

- Browser: `http://127.0.0.1:8765`
- Other containers: `http://legacy:8765`
- `GET /api/tables`
- `GET /api/rows?table=TABLE&limit=50&offset=0&q=`

`make dev` does not start this service. `LICO` contains personal data; do not export it wholesale. CloudBeaver, FlameRobin, and FirebirdWebAdmin need a real InterBase server and cannot attach to this container.

Code lives in `tools/interbase/`. The Compose service name is `legacy`, image `buy-legacy-gdb`, profile `legacy`.
