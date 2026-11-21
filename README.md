
#!/usr/bin/env bash
set -euo pipefail

# Layer 18: Malware Protection Status (Read-Only Audit Helper)
# Outputs structured JSON with status: PASS | WARN | FAIL

xprotect_version="unknown"
if [ -f "/System/Library/CoreServices/XProtect.bundle/Contents/Info.plist" ]; then
  xprotect_version=$(/usr/libexec/PlistBuddy -c "Print :CFBundleShortVersionString" \
    "/System/Library/CoreServices/XProtect.bundle/Contents/Info.plist" 2>/dev/null || echo "present")
fi

gatekeeper_status=$(spctl --status 2>/dev/null || echo "unknown")

# Check for potentially suspicious/unsigned scripts or binaries in ~/Downloads
suspicious_downloads=()
if [ -d "$HOME/Downloads" ]; then
  while IFS= read -r -d '' file; do
    suspicious_downloads+=("$file")
  done < <(find "$HOME/Downloads" -maxdepth 2 -type f \( -name "*.command" -o -name "*.pkg" -o -name "*.dmg" -o -name "*.sh" \) -print0 2>/dev/null)
fi

status="PASS"
summary="XProtect (${xprotect_version}) active; Gatekeeper (${gatekeeper_status})."

if [[ "$gatekeeper_status" != *"enabled"* ]]; then
  status="FAIL"
  summary="Gatekeeper is not enabled (${gatekeeper_status})."
elif [ "${#suspicious_downloads[@]}" -gt 0 ]; then
  status="WARN"
  summary="Found ${#suspicious_downloads[@]} installer/script item(s) in ~/Downloads to review."
fi

cat <<EOF
{
  "layer": 18,
  "name": "Malware protection status",
  "status": "${status}",
  "xprotect_version": "${xprotect_version}",
  "gatekeeper": "${gatekeeper_status}",
  "downloads_flagged_count": ${#suspicious_downloads[@]},
  "summary": "${summary}"
}
EOF
