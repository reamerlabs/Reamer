# REAMER

**A local-first backtesting and research engine for systematic OHLCV traders.**

All computation runs on your machine. No cloud, no rate limits, no data uploads.


## Downloads

Go to the [Releases](../../releases/latest) tab and download the file for your platform.

| Platform | File |
|---|---|
| Linux x86-64 | `reamer-gui-linux-x86_64.AppImage` |
| macOS Apple Silicon | `reamer-gui-macos-arm64.dmg` |
| macOS Intel | `reamer-gui-macos-x86_64.dmg` |
| Windows x86-64 | `reamer-gui-windows-x86_64.zip` |
| Python wheel | `reamer_py-*.whl` |

---

## Requirements

- **Python 3.10 or later** — required for running strategies
- A valid REAMER license key (purchase at [reamerlabs.com](https://reamerlabs.com))

---

## Installation

**Linux**
```bash
chmod +x reamer-gui-linux-x86_64.AppImage
./reamer-gui-linux-x86_64.AppImage
```

macOS

Open the .dmg and drag REAMER to your Applications folder. On first launch, right-click → Open to bypass Gatekeeper (the app is currently unsigned).


Windows

Extract the .zip and run reamer_gui.exe. No installer needed.


---
License Activation

On first launch the app shows an activation dialog. Enter your license key there.

To activate from the command line (e.g. on a headless server), use the bundled reamer_activate tool:

# Activate
```bash
reamer_activate YOUR-LICENSE-KEY
```

# Deactivate (frees the seat so you can move to another machine)
```bash
reamer_activate --deactivate
```

The license covers one machine at a time. To move to a new machine, deactivate on the old one first. If you no longer have access to the original machine, email support@reamerlabs.com from your registered email to release the seat manually.

---
Python Strategy SDK

Write a Python class, return order dicts from on_candle, point REAMER at it.

```python
from engine.orders import buy_market, close_position

class MyStrategy:
    def __init__(self, config_path=None):
        self.in_pos = False

    def on_candle(self, data):
        tv = data[data.tickers[0]]
        if not tv.valid:
            return None
        if float(tv.close) > float(tv.open) and not self.in_pos:
            self.in_pos = True
            return buy_market(10.0, ticker=data.tickers[0])
        if float(tv.close) < float(tv.open) and self.in_pos:
            self.in_pos = False
            return close_position(ticker=data.tickers[0])
```

To install the Python module directly:

```bash
pip install reamer_py-*.whl
```

The SDK supports single-asset and multi-asset strategies, long/short, market/limit/stop orders, TP/SL brackets, IOC/GTD time-in-force, partial closes, scale-ins, reversals, leverage, overnight swap, and configurable spread/slippage/commission.

---
What's included

- GUI — load CSV data, configure and run backtests, inspect trades on an interactive equity chart, step through the replay tick-by-tick, run Monte Carlo simulations, export PDF reports
- Python module (reamer_py) — embed backtests in your own scripts and notebooks
- reamer_activate — headless license activation/deactivation CLI

---
Support

support@reamerlabs.com
