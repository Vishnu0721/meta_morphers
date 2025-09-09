## Qubit Bloch Visualizer

Web app to load OpenQASM 2.0 circuits, simulate the statevector, compute single-qubit reduced density matrices via partial trace, and plot each qubit on a Bloch sphere.

### Run web (static, no install)
- Open `web/index.html` in any modern browser

### Run Python live server (optional, dynamic inputs)
1. Create venv and install deps
```bash
py -m venv .venv
.\.venv\Scripts\python -m pip install --upgrade pip
.\.venv\Scripts\pip install -r requirements.txt
```
2. Start API
```bash
.\.venv\Scripts\uvicorn server.main:app --reload --port 8000
```
3. In the web app, choose Mode: "Live data (URL)" and set URL to `http://127.0.0.1:8000/api/frame` then click Connect
4. To animate a custom circuit, POST your QASM to prepare a sequence of frames:
```bash
curl -X POST http://127.0.0.1:8000/api/prepare -H "Content-Type: application/json" \
  -d '{"qasm":"OPENQASM 2.0;\ninclude \"qelib1.inc\";\nqreg q[2];\nh q[0];\ncx q[0],q[1];\n", "frames_per_step": 30}'
```
Then refresh/keep connecting; the `/api/frame` endpoint cycles frames.

### Notes
- Only unitary circuits are supported for statevector simulation; measurement ops are ignored.
- Bloch vectors use convention `r = (Tr(ρσx), Tr(ρσy), Tr(ρσz))`.

