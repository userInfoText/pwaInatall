# AGENTS

## Local HTTP services

This project uses two local `http-server` processes when testing the PWA on a phone.

### Port 8080: PWA wrapper

- URL: `http://192.168.1.30:8080`
- Directory: `c:\Users\liang\Desktop\pwa`
- Purpose: serves the PWA install page, manifest, icons, service worker, and wrapper `index.html`.
- Main test URL: `http://192.168.1.30:8080/install.html?channel=0`
- Manifest URL: `http://192.168.1.30:8080/manifest/0/installmanifest.json`

Start command:

```powershell
node C:\Users\liang\AppData\Roaming\npm\node_modules\http-server\bin\http-server c:\Users\liang\Desktop\pwa -a 0.0.0.0 -p 8080 -c-1
```

### Port 8081: game build

- URL: `http://192.168.1.30:8081`
- Directory: `D:\newGame\build\web-mobile`
- Purpose: serves the actual game build loaded by the PWA wrapper iframe.
- Main game URL: `http://192.168.1.30:8081/index.html`
- Config reference: `M_ChannelApkUrlCFG.json` points `PwaUrl` and `PwaTestUrl` to this URL.

Start command:

```powershell
node C:\Users\liang\AppData\Roaming\npm\node_modules\http-server\bin\http-server D:\newGame\build\web-mobile -a 0.0.0.0 -p 8081 -c-1
```

## Quick checks

```powershell
Invoke-WebRequest -UseBasicParsing http://192.168.1.30:8080/install.html?channel=0
Invoke-WebRequest -UseBasicParsing http://192.168.1.30:8081/index.html
Get-NetTCPConnection -LocalPort 8080,8081
```

## Notes

- `8080` is the address users install as the PWA. The manifest `start_url` and `scope` should stay on `http://192.168.1.30:8080`.
- `8081` is only the inner game content server. The wrapper page on `8080` loads it through `M_ChannelApkUrlCFG.json`.
- Chrome on Android must treat `http://192.168.1.30:8080` as a secure origin, or the install flow may fall back to a normal shortcut.
- If the manifest changes, delete the old desktop icon and install again so Chrome reads the new manifest.
