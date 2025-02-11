# rzls Crawler

This repository will crawl the [msft_consumption feed](https://dev.azure.com/azure-public/vside/_artifacts/feed/msft_consumption/NuGet/rzls.linux-x64/overview)
for new releases of the rzls to be used with [rzls.nvim](https://github.com/tris203/rzls.nvim)
and uploads them as artifacts as github releases

## AutoUpdate for nvim

### Mason

In case you're already using [mason](https://github.com/williamboman/mason.nvim) you can add a custom registry.

```lua
  {
    "williamboman/mason.nvim",
    cmd = {
      "Mason",
      "MasonInstall",
      "MasonUninstall",
      "MasonUninstallAll",
      "MasonLog",
      "MasonUpdate",
    },
    opts = {
      registries = {
        "github:mason-org/mason-registry",
        "github:crashdummyy/mason-registry"
      }
    }
  },

```

or  

```lua
require('mason').setup({
        registries = {
            'github:mason-org/mason-registry',
            'github:crashdummyy/mason-registry'
        },
    })
```

This registry currently serves at least the languageServer for [roslyn.nvim](https://github.com/seblj/roslyn.nvim) and [rzls.nvim](https://github.com/tris203/rzls.nvim)

This registry currently serves at least the languageServer for [roslyn.nvim](https://github.com/seblj/roslyn.nvim) and [rzls.nvim](https://github.com/tris203/rzls.nvim)

### Linux ( manual )

The variable `rid` might need to be altered

```bash
#!/bin/bash

if ! command -v unzip &> /dev/null
then
    echo "unzip is required. Please install it"
    exit 1
fi

rid="linux-x64"
targetDir="$HOME/.local/share/nvim/rzls"
latestVersion=$(curl -s https://api.github.com/repos/Crashdummyy/rzls/releases | grep tag_name | head -1 | cut -d '"' -f4)

[[ -z "$latestVersion" ]] && echo "Failed to fetch the latest package information." && exit 1

echo "Latest version: $latestVersion"

asset=$(curl -s https://api.github.com/repos/Crashdummyy/rzls/releases | grep "releases/download/$latestVersion" | grep "$rid"| cut -d '"' -f 4)

echo "Downloading: $asset"

curl -Lo "./rzls.zip" "$asset"

echo "Remove old installation"
rm -rf $targetDir/*

unzip "./rzls.zip" -d "$targetDir/"
rm "./rzls.zip"
```

### Macos

TBD

### Windows ( manual )

#### x64

```powershell
$file = New-Guid
Invoke-WebRequest https://github.com/Crashdummyy/rzls/releases/latest/download/rzls.win-x64.zip -OutFile ~/Downloads/$file.zip
Expand-Archive ~/Downloads/$file.zip -DestinationPath ~/AppData/Local/nvim-data/rzls/ -Force
rm ~/Downloads/$file.zip
```

#### arm64

```powershell
$file = New-Guid
Invoke-WebRequest https://github.com/Crashdummyy/rzls/releases/latest/download/rzls.win-arm64.zip -OutFile ~/Downloads/$file.zip
Expand-Archive ~/Downloads/$file.zip -DestinationPath ~/AppData/Local/nvim-data/rzls/ -Force
rm ~/Downloads/$file.zip
```
