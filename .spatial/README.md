# Learning pipelinewise with Spatial Code

This checkout uses the sibling `../spatial-code` installation. Set `SPATIAL_CODE=/absolute/path/to/spatial-code` if yours is elsewhere. Install the engine dependencies once in that checkout (`cd engine && npm install`). The interactive view also needs Spatial Code's viewer dependencies and Coruscant configured as described in its README.

```sh
./spatial-map                  # open/reuse this repository's isolated explorer
./spatial-map build            # refresh a local map without starting a server
./spatial-map verify           # check the generated map
./spatial-map query domains   # ask a structural question in the terminal
```

The interactive explorer builds its own current map on start. If it is already running after source changes, restart its `wise-pipelinewise` instance in Coruscant to refresh it. `build` writes `.spatial/map/`, which is ignored by Git; it does not refresh an already running explorer. The map reads source without running the application. Runtime behavior must still be checked against source and tests.

## Trace an ingestion run

1. Start with `pipelinewise/cli/__init__.py`: find command parsing and dispatch.
2. Follow `pipelinewise/cli/pipelinewise.py` into `pipelinewise/cli/commands.py`. Pick one run path and inspect its callers and dependencies.
3. Read `pipelinewise/cli/config.py`, then follow the chosen connector/fast-sync path. Ask where state is stored, how a retry resumes, and which failures propagate.
4. Use the source tests and documentation alongside the map to verify recovery behavior. Connector dependencies and SQL are not fully represented.

In the explorer, start at Overview, open an area, then open a file. Inspect its dependencies, users, symbols and source. Pick one public operation and write down the request path, state transitions, failure handling and ownership boundaries. Treat inferred links and blind spots as prompts to inspect the code.

Python imports and statically resolvable names are mapped. Dynamic calls, framework wiring and external dependencies can be incomplete. Tests and notebooks are outside the source map.
