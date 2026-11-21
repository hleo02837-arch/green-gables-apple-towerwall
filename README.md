<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Green Gables Apple Towerwall</title>
  <style>
    :root {
      color-scheme: dark;
      font-family: system-ui, sans-serif;
      background: #10251d;
      color: #f2f5ef;
    }
    body { max-width: 760px; margin: 32px auto; padding: 20px; }
    main, li { background: #1b382c; padding: 16px; border-radius: 10px; }
    li { margin: 10px 0; }
    .pass { color: #9be6a8; }
    .fail { color: #ff9a8c; }
    .unknown { color: #ffd778; }
    .notice { color: #d2ded6; }
  </style>
</head>
<body>
  <h1>Green Gables Apple Towerwall</h1>
  <p class="notice">
    This page displays a report created by the read-only macOS audit script.
    It does not scan your Mac itself. No example results are included.
  </p>

  <label for="report">Select an audit report (.json):</label>
  <input id="report" type="file" accept=".json,application/json">

  <main id="results" aria-live="polite">No report loaded.</main>

  <script>
    const input = document.querySelector("#report");
    const results = document.querySelector("#results");

    input.addEventListener("change", async () => {
      results.replaceChildren();

      if (!input.files.length) {
        results.textContent = "No report loaded.";
        return;
      }

      try {
        const report = JSON.parse(await input.files[0].text());

        if (!Array.isArray(report.checks)) {
          throw new Error("The report does not contain a checks list.");
        }

        const heading = document.createElement("p");
        heading.textContent = report.generated_at
          ? `Report generated: ${report.generated_at}`
          : "Report date not provided.";
        results.append(heading);

        for (const check of report.checks) {
          const item = document.createElement("li");
          const status = ["pass", "fail", "unknown"].includes(check.status)
            ? check.status
            : "unknown";

          item.className = status;
          item.textContent =
            `${check.name || "Unnamed check"}: ${status.toUpperCase()} — ` +
            `${check.detail || "No details provided"}`;
          results.append(item);
        }
      } catch {
        results.textContent =
          "Could not read a valid audit report. Please select the JSON report created by the audit script.";
      }
    });
  </script>
</body>
</html>
