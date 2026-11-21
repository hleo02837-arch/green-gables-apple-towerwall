<!doctype html>
<html lang="en">
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Green Gables Apple Towerwall</title>
<style>
body{font:16px system-ui;background:#10251d;color:#f2f5ef;max-width:760px;margin:40px auto;padding:20px}
main,li{background:#1b382c;padding:16px;border-radius:10px;margin:12px 0}
.pass{color:#9be6a8}.fail{color:#ff9a8c}.unknown{color:#ffd778}
</style>
<h1>Green Gables Apple Towerwall</h1>
<p>Choose a report produced by the read-only audit script. No results are shown until you load one.</p>
<input id="file" type="file" accept=".json,application/json">
<main id="results">No report loaded.</main>
<script>
document.querySelector("#file").onchange = async e => {
  const box = document.querySelector("#results");
  try {
    const data = JSON.parse(await e.target.files[0].text());
    if (!Array.isArray(data.checks)) throw Error("Invalid report format");
    box.replaceChildren();
    for (const c of data.checks) {
      const item = document.createElement("li");
      item.className = c.status;
      item.textContent = `${c.name}: ${c.status.toUpperCase()} — ${c.detail}`;
      box.append(item);
    }
  } catch {
    box.textContent = "Could not read a valid Towerwall report.";
  }
};
</script>
</html>
