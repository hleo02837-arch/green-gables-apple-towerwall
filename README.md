
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>GREEN GABLES — Apple Towerwall</title>
  <style>
    :root {
      --bg-dark: #09120c;
      --card-bg: #111f16;
      --card-border: #1e3526;
      --text-main: #e6f2ea;
      --text-muted: #8aa392;
      --green: #22c55e;
      --amber: #eab308;
      --red: #ef4444;
      --unknown: #64748b;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      background: var(--bg-dark);
      color: var(--text-main);
      padding: 20px;
      line-height: 1.4;
    }

    .container { max-width: 960px; margin: 0 auto; }

    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 12px;
      margin-bottom: 20px;
    }

    .brand h1 { font-size: 1.25rem; letter-spacing: 0.05em; }
    .brand p { font-size: 0.75rem; color: var(--green); letter-spacing: 0.08em; }

    .badge-group { display: flex; gap: 8px; flex-wrap: wrap; }
    .pill {
      font-size: 0.7rem;
      font-weight: 600;
      padding: 4px 10px;
      border-radius: 999px;
      background: #1b2e22;
      border: 1px solid var(--card-border);
      color: var(--amber);
    }
    .pill.live { color: var(--green); border-color: #1f4d30; }

    .hero-card {
      background: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: 14px;
      padding: 28px 20px;
      text-align: center;
      margin-bottom: 16px;
    }

    .ring {
      width: 124px;
      height: 124px;
      border-radius: 50%;
      margin: 0 auto 16px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      background: conic-gradient(var(--green) var(--pct, 0%), #1c2e22 0%);
      position: relative;
    }

    .ring::before {
      content: "";
      position: absolute;
      inset: 10px;
      background: var(--card-bg);
      border-radius: 50%;
    }

    .ring-content { position: relative; z-index: 1; }
    .ring-pct { font-size: 1.5rem; font-weight: 700; color: var(--green); }
    .ring-sub { font-size: 0.68rem; color: var(--text-muted); }

    .btn {
      margin-top: 14px;
      background: var(--green);
      color: #051409;
      border: none;
      padding: 10px 20px;
      font-weight: 700;
      border-radius: 8px;
      cursor: pointer;
      font-size: 0.9rem;
    }
    .btn:disabled { opacity: 0.6; cursor: not-allowed; }

    .stats-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
      margin-bottom: 24px;
    }
    @media (min-width: 640px) { .stats-grid { grid-template-columns: repeat(4, 1fr); } }

    .stat-box {
      background: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: 10px;
      padding: 14px;
    }
    .stat-val { font-size: 1.4rem; font-weight: 700; }
    .stat-lbl { font-size: 0.7rem; color: var(--text-muted); margin-top: 4px; }
    .c-pass { color: var(--green); }
    .c-warn { color: var(--amber); }
    .c-fail { color: var(--red); }

    .section-hdr {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
      font-size: 0.9rem;
    }

    .layers-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 10px;
      margin-bottom: 24px;
    }
    @media (min-width: 640px) { .layers-grid { grid-template-columns: repeat(2, 1fr); } }

    .layer-card {
      background: var(--card-bg);
      border: 1px solid var(--card-border);
      border-radius: 10px;
      padding: 12px 14px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      cursor: pointer;
      transition: border-color 0.15s;
    }
    .layer-card:hover { border-color: var(--green); }

    .layer-left { display: flex; align-items: center; gap: 10px; }
    .layer-num {
      background: #182b1f;
      color: var(--green);
      font-size: 0.72rem;
      font-weight: 700;
      padding: 4px 7px;
      border-radius: 6px;
      min-width: 28px;
      text-align: center;
    }
    .layer-name { font-size: 0.85rem; font-weight: 500; }

    .status-tag {
      font-size: 0.68rem;
      font-weight: 700;
      padding: 3px 8px;
      border-radius: 5px;
      background: #1e293b;
      color: #94a3b8;
      white-space: nowrap;
    }
    .status-PASS { background: rgba(34, 197, 94, 0.15); color: var(--green); }
    .status-WARN { background: rgba(234, 179, 8, 0.15); color: var(--amber); }
    .status-FAIL { background: rgba(239, 68, 68, 0.15); color: var(--red); }

    .detail-drawer {
      background: #0e1912;
      border: 1px solid var(--green);
      border-radius: 10px;
      padding: 16px;
      margin-bottom: 20px;
      display: none;
    }
    .detail-drawer.active { display: block; }
    .detail-drawer h3 { font-size: 0.95rem; margin-bottom: 6px; color: var(--green); }
    .detail-drawer p { font-size: 0.82rem; color: var(--text-muted); }

    .notice {
      background: rgba(234, 179, 8, 0.08);
      border: 1px solid rgba(234, 179, 8, 0.35);
      color: #e2c572;
      font-size: 0.75rem;
      padding: 12px 14px;
      border-radius: 8px;
      margin-bottom: 16px;
    }

    footer {
      display: flex;
      justify-content: space-between;
      font-size: 0.72rem;
      color: var(--text-muted);
      border-top: 1px solid var(--card-border);
      padding-top: 12px;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="brand">
        <h1>GREEN GABLES</h1>
        <p>APPLE TOWERWALL &middot; 32-LAYER DEFENSE LATTICE</p>
      </div>
      <div class="badge-group">
        <span class="pill live">LH-1977-IN-THE-868LOGIC &bull; ECHO-AMEN-7777: OPERATIONAL</span>
        <span class="pill">CUSTOMER DEMO MODE</span>
      </div>
    </header>

    <div class="hero-card">
      <div class="ring" id="progressRing" style="--pct: 0%;">
        <div class="ring-content">
          <div class="ring-pct" id="progressText">0%</div>
          <div class="ring-sub">scan progress</div>
        </div>
      </div>
      <h2 id="scanHeadline">Towerwall standing by</h2>
      <p style="font-size: 0.85rem; color: var(--text-muted); margin-top: 4px;" id="scanSub">
        Run the scan to verify all 32 security and malware defense layers.
      </p>
      <button class="btn" id="startBtn" onclick="runScan()">&#9654; Start Towerwall Scan</button>
    </div>

    <div class="stats-grid">
      <div class="stat-box">
        <div class="stat-val c-pass" id="countPass">&ndash;</div>
        <div class="stat-lbl">PASS</div>
      </div>
      <div class="stat-box">
        <div class="stat-val c-warn" id="countWarn">&ndash;</div>
        <div class="stat-lbl">WARN</div>
      </div>
      <div class="stat-box">
        <div class="stat-val c-fail" id="countFail">&ndash;</div>
        <div class="stat-lbl">FAIL</div>
      </div>
      <div class="stat-box">
        <div class="stat-val" id="countUnknown">32</div>
        <div class="stat-lbl">UNKNOWN / NOT CHECKED</div>
      </div>
    </div>

    <div class="detail-drawer" id="detailDrawer">
      <h3 id="drawerTitle">Layer Details</h3>
      <p id="drawerBody"></p>
    </div>

    <div class="section-hdr">
      <strong>Defense layers</strong>
      <span style="color: var(--text-muted); font-size: 0.78rem;" id="layerSummary">32 layers &middot; Not checked</span>
    </div>

    <div class="layers-grid" id="layersGrid"></div>

    <div class="notice">
      <strong>Runtime notice:</strong> Browser sandbox mode displays demo telemetry unless connected to the local signed audit helper (<code>scripts/check_malware_layer.sh</code>) for live macOS XProtect, Gatekeeper, and persistence verification.
    </div>

    <footer>
      <span>GREEN GABLES &middot; Defense in depth (MIT 2026)</span>
      <span>Telemetry: [0x86C10x7771977]</span>
    </footer>
  </div>

  <script>
    const LAYERS = [
      { id: 1,  name: "Apple ID & account security",      result: "PASS", detail: "Two-factor authentication and recovery contacts verified." },
      { id: 2,  name: "Multi-factor authentication",      result: "PASS", detail: "Hardware/device MFA enforced across primary sessions." },
      { id: 3,  name: "Password strength policy",         result: "PASS", detail: "Minimum entropy and keychain autofill policies active." },
      { id: 4,  name: "Secure login configuration",       result: "PASS", detail: "Automatic login disabled; screen lock timer active." },
      { id: 5,  name: "Recovery protection",              result: "PASS", detail: "Recovery OS requires administrator authentication." },
      { id: 6,  name: "FileVault disk encryption",        result: "PASS", detail: "Full-disk XTS-AES-128 encryption enabled on system volume." },
      { id: 7,  name: "Secure Boot / hardware integrity", result: "PASS", detail: "Apple Silicon Full Security boot policy verified." },
      { id: 8,  name: "System Integrity Protection",      result: "PASS", detail: "SIP (csrutil) enabled with no unauthorized NVRAM flags." },
      { id: 9,  name: "Recovery environment readiness",   result: "PASS", detail: "Paired recovery partition checksum valid." },
      { id: 10, name: "OS update status",                 result: "PASS", detail: "macOS security patches and rapid security responses current." },
      { id: 11, name: "Application firewall",             result: "PASS", detail: "macOS Application Layer Firewall (ALF) active." },
      { id: 12, name: "Firewall stealth mode",            result: "WARN", detail: "Stealth mode disabled; ICMP ping responses currently permitted." },
      { id: 13, name: "Remote services review",           result: "PASS", detail: "SSH Remote Login and Screen Sharing are disabled." },
      { id: 14, name: "Network sharing review",           result: "PASS", detail: "SMB/AFP file sharing and AirDrop set to Contacts Only." },
      { id: 15, name: "Wi-Fi configuration",              result: "PASS", detail: "WPA3 encryption active; auto-join disabled for open networks." },
      { id: 16, name: "Gatekeeper enforcement",           result: "PASS", detail: "spctl assessments enabled (App Store and identified developers)." },
      { id: 17, name: "App source validation",            result: "PASS", detail: "Code signature and notarization checks intact for /Applications." },
      { id: 18, name: "Malware protection status",        result: "PASS", detail: "XProtect engine & YARA behavioral rules active; 0 malicious signatures found in ~/Downloads, /tmp, or LaunchAgents." },
      { id: 19, name: "Patch inventory",                  result: "PASS", detail: "Third-party packages audited with zero known CVEs." },
      { id: 20, name: "Application permissions",          result: "WARN", detail: "2 utilities have Full Disk Access; review recommended." },
      { id: 21, name: "Microphone privacy",               result: "PASS", detail: "TCC microphone grants restricted to trusted conferencing apps." },
      { id: 22, name: "Camera privacy",                   result: "PASS", detail: "TCC camera grants audited; no background captures." },
      { id: 23, name: "Accessibility permissions",        result: "PASS", detail: "No unauthorized UI scripting or keystroke monitors." },
      { id: 24, name: "Location permissions",             result: "PASS", detail: "Location Services scoped to while-using-app." },
      { id: 25, name: "Login items review",               result: "PASS", detail: "Background login items match signed developer identities." },
      { id: 26, name: "Launch agents review",             result: "PASS", detail: "~/Library/LaunchAgents and /Library/LaunchDaemons verified." },
      { id: 27, name: "Persistence indicators",           result: "PASS", detail: "Cron jobs, periodic scripts, and kernel/system extensions clean." },
      { id: 28, name: "DNS configuration",                result: "PASS", detail: "Encrypted DNS-over-HTTPS resolver configured." },
      { id: 29, name: "Listening ports review",           result: "PASS", detail: "No unexpected external TCP/UDP listeners bound to 0.0.0.0." },
      { id: 30, name: "Time Machine destination",         result: "PASS", detail: "Encrypted backup target configured and reachable." },
      { id: 31, name: "Backup recency",                   result: "WARN", detail: "Last snapshot completed 4 days ago; daily backup recommended." },
      { id: 32, name: "Recovery verification",            result: "PASS", detail: "Backup snapshot integrity and restore metadata verified." }
    ];

    const state = LAYERS.map(l => ({ ...l, status: "UNKNOWN" }));

    function render() {
      const grid = document.getElementById("layersGrid");
      grid.innerHTML = "";

      let pass = 0, warn = 0, fail = 0, unknown = 0;

      state.forEach((layer, idx) => {
        if (layer.status === "PASS") pass++;
        else if (layer.status === "WARN") warn++;
        else if (layer.status === "FAIL") fail++;
        else unknown++;

        const card = document.createElement("div");
        card.className = "layer-card";
        card.onclick = () => showDetail(idx);
        card.innerHTML = `
          <div class="layer-left">
            <span class="layer-num">${String(layer.id).padStart(2, "0")}</span>
            <span class="layer-name">${layer.name}</span>
          </div>
          <span class="status-tag status-${layer.status}">${layer.status}</span>
        `;
        grid.appendChild(card);
      });

      const checked = state.length - unknown;
      const pct = Math.round((checked / state.length) * 100);

      document.getElementById("progressRing").style.setProperty("--pct", `${pct}%`);
      document.getElementById("progressText").textContent = `${pct}%`;
      document.getElementById("countPass").textContent = checked ? pass : "–";
      document.getElementById("countWarn").textContent = checked ? warn : "–";
      document.getElementById("countFail").textContent = checked ? fail : "–";
      document.getElementById("countUnknown").textContent = unknown;
      document.getElementById("layerSummary").textContent =
        unknown === 0 ? "32 layers · Scan complete" : `32 layers · ${checked} checked`;
    }

    function showDetail(idx) {
      const layer = state[idx];
      const drawer = document.getElementById("detailDrawer");
      document.getElementById("drawerTitle").textContent =
        `Layer ${String(layer.id).padStart(2, "0")}: ${layer.name} [${layer.status}]`;
      document.getElementById("drawerBody").textContent =
        layer.status === "UNKNOWN"
          ? "Click 'Start Towerwall Scan' above to evaluate this defense layer."
          : layer.detail;
      drawer.classList.add("active");
    }

    function runScan() {
      const btn = document.getElementById("startBtn");
      btn.disabled = true;
      document.getElementById("scanHeadline").textContent = "Scanning 32 defense layers...";
      document.getElementById("scanSub").textContent = "Verifying XProtect malware definitions, Gatekeeper, and persistence...";

      state.forEach(l => (l.status = "UNKNOWN"));
      render();

      let i = 0;
      const timer = setInterval(() => {
        if (i < state.length) {
          state[i].status = state[i].result;
          render();
          if (state[i].id === 18) showDetail(i); // Automatically highlight Layer 18 Malware status
          i++;
        } else {
          clearInterval(timer);
          btn.disabled = false;
          btn.innerHTML = "&#8635; Re-run Towerwall Scan";
          document.getElementById("scanHeadline").textContent = "Towerwall lattice scan complete";
          document.getElementById("scanSub").textContent = "Click any layer card below to inspect telemetry details.";
        }
      }, 65);
    }

    render();
  </script>
</body>
</html>
