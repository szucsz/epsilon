# Building Epsilon in Dev Container

This dev container includes all dependencies needed to build the Epsilon graphing calculator simulator for Linux.

## Quick Start

### 1. Build the Simulator

From the workspace root:

```bash
make -C epsilon PLATFORM=simulator clean
make -C epsilon -j8 PLATFORM=simulator epsilon.bin
```

The compiled binary will be at: `epsilon/output/release/linux/epsilon.bin`

### 2. Set Up X11 Display Forwarding (macOS)

To run the graphical simulator from the Linux container and display it on macOS:

**On your macOS host:**

1. Install and start XQuartz (if not already installed)
2. In XQuartz Settings > Security, enable: "Allow connections from network clients"
3. Restart XQuartz
4. Allow local connections:
   ```bash
   /usr/X11/bin/xhost +localhost
   /usr/X11/bin/xhost +127.0.0.1
   ```

**In the container:**

```bash
export DISPLAY=host.docker.internal:0
/workspaces/epsilon/epsilon/output/release/linux/epsilon.bin
```

The Epsilon calculator UI will appear in an XQuartz window on your macOS screen.

## Build Targets

- `epsilon.bin` — Release build for Linux simulator
- `epsilon.compare` — Run screenshot comparison tests
- `epsilon_test.bin` — Unit tests

Example:
```bash
make -C epsilon PLATFORM=simulator epsilon_test.bin
```

## Notes

- **First build may take 5-10 minutes** as it compiles all dependencies
- **Rebuild is faster** — incremental compilation
- The Linux platform uses `.bin` extension (macOS uses `.app`)
- For device builds (ARM firmware), use `PLATFORM=n0120` etc., but requires ARM toolchain

## Troubleshooting

**App crashes on launch?**
- Verify DISPLAY is set: `echo $DISPLAY`
- Check XQuartz is running and has network access enabled
- Try: `xhost +` on macOS (less secure, wider access)

**X11 connection refused?**
- Restart XQuartz
- Re-run `xhost +localhost` on macOS
- Try `host.containers.internal` instead if `host.docker.internal` fails

**Build errors?**
- Clean rebuild: `make -C epsilon PLATFORM=simulator clean && make -C epsilon -j8 PLATFORM=simulator epsilon.bin`
- Check `/epsilon/output/release/linux/` for build artifacts

## Further Reading

- [Main build docs](../epsilon/docs/build/README.md)
- [Epsilon architecture](../epsilon/docs/README.md)
