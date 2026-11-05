# Shader Cache Database

A community-driven repository providing pre-compiled Fossilize shader pipeline databases (`.foz` / `.foz.zipped`) and DXVK/VKD3D shader caches for popular Steam games. Optimized to eliminate in-game micro-stutters,dxvk/vkd3d translation overhead and shader compilation lag across Linux (Proton/Steam Deck) and Windows.



## 🎯 Purpose

Modern graphics APIs and heavy games frequently compile shaders on-the-fly during active gameplay, causing severe stuttering and frame drops.
This repository archives [Fossilize](https://github.com/ValveSoftware/Fossilize) pipeline states recorded by steam. By pre-loading these cached binaries, users can bypass runtime compilation entirely, ensuring smooth frame pacing from the very first launch.

---

## 🪟 Platforms & Compatibility

Because Fossilize captures intermediary Vulkan and driver pipeline state metadata rather than game engine code, **these `.foz` databases are strictly game-specific while remaining completely GPU, OS, and platform-agnostic**.

As a result, pre-compiled pipeline caches hosted here are not restricted to the Steam client. They can be replayed and injected across non-Steam environments, including:
- **GOG & Epic Games Store** releases (via Heroic, Lutris, or standalone launchers)
- **Custom / Emulated Steam API environments** (e.g., Goldberg Emulator / `gbe_fork`)


---

## 🚀 How to Use

###
You do **not** need Steam running to inject these caches. Valve includes a standalone replay tool (`fossilize-replay`) in steam download directory that parses `.foz` files and injects compiled pipelines directly into your GPU driver's native cache (`NVIDIA DXCache` / `AMD VkCache`).

Valve also provides the official [**Fossilize**](https://github.com/ValveSoftware/Fossilize) toolset (`fossilize-replay` and `fossilize-valgrind` / recording hooks), which operates entirely independently of Steam intsallation.


### 1. Download the game's corresponding `.foz` or `.foz.zipped` file from the [Releases page](../../releases) and extract the archive.
### 2. Replaying Caches (`fossilize-replay`)
- **🪟 Windows (PowerShell):**
  ```powershell
  # Set environment variables to route output to your staging folder
  $env:__GL_SHADER_DISK_CACHE_PATH="$PWD"  # For NVIDIA
  $env:MESA_SHADER_CACHE_DIR="$PWD"        # For AMD/Mesa

  # Run the replayer tool with whitelist and progress tracking
  & "C:\Program Files (x86)\Steam\bin\fossilize_replay.exe" --progress --on-disk-replay-whitelist "path\to\steam_pipeline_cache_whitelist.foz" "path\to\steam_pipeline_cache.foz"
  ```

- **🐧 Linux Terminal:**
  ```bash
  # Set environment variables to route output to your staging folder
  export __GL_SHADER_DISK_CACHE_PATH="./.foz/download" # For NVIDIA
  export MESA_SHADER_CACHE_DIR="./.foz/download"        # For AMD/Mesa

  # Run the replayer tool with whitelist and progress tracking
  ~/.local/share/Steam/ubuntu12_64/fossilize_replay  --progress --on-disk-replay-whitelist "path\to\steam_pipeline_cache_whitelist.foz" "path\to\steam_pipeline_cache.foz"
  ```
### 3. Moving the output cache directory
- **Option A: Move to Driver Default Cache Directory (Global):**
> [!NOTE]
> Without mentioning the cache path in step 2,fossilize-replay will put the cache directly into the driver default directory.

  * **NVIDIA Windows:** `C:\Users\<YourUsername>\AppData\Local\NVIDIA\DXCache\`
  * **NVIDIA Linux:** `~/.cache/nvidia/GLCache/`
  * **AMD Windows:** `C:\Users\<YourUsername>\AppData\Local\AMD\VkCache\`
  * **AMD Linux:** `~/.cache/mesa_shader_cache/`
  * **⚠️ Important (Disable Auto-Cleanup):** Graphics drivers periodically purge unused cache bins. Make to increase cache maximum size or disable auto cache delete.
---


- **Option B: Move to a Game-Specific Folder + Runtime Environment Variable (Recommended & Clean)**
  - **🟢 NVIDIA Configuration:**
    - *Windows (PowerShell):*
    ```powershell
    $env:__GL_SHADER_DISK_CACHE_PATH="C:\path\to\game\custom_shader_cache"
    $env:__GL_SHADER_DISK_CACHE="1"
    .\game.exe
    ```
    - *Linux:*
    ```bash
    export __GL_SHADER_DISK_CACHE_PATH="/path/to/game/custom_shader_cache"
    export __GL_SHADER_DISK_CACHE=1
    ./game_binary
    ```

  - **🔴 AMD / Mesa Configuration:**
    - *Windows (PowerShell):*
    ```powershell
    $env:MESA_SHADER_CACHE_DIR="/path/to/game/custom_shader_cache"
    $env:MESA_SHADER_CACHE="1"
    .\game.exe
    ```
    - *Linux :*
    ```bash
    export MESA_SHADER_CACHE_DIR="/path/to/game/custom_shader_cache"
    export MESA_SHADER_CACHE=1
    ./game_binary
    ```
---

## 📤 Want to Contribute (Submit a Cache!)
1. You need to own the game at steam.Enable shader pre-caching on steam at `Settings > Downloads > Shader Pre-Caching > Enable Shader Pre-cache`
2. Download the base game (at minimum),launch the game,steam will download shader pre-cache in the background at first launch.
3. Without enabling shader pre-cache and downloading at least the base game, steam will not download shader pre-cache files.It is not required to download any DLC as steam bundles shader pre-cache per game basis.Any time a game updates and the graphics pipeline changes steam updates these shader files.
4. Upload your files into the submission form: [Google Form](https://forms.gle/By3EVEbef5e7B6Zk8) or host the files in your desired platform and make a request.Submission process is explained in detail in the form.
5. **Verification:** The integrity of the database will be verified locally before packaging it into official GitHub Releases.
---

## DXVK/VKD3D Shader Cache (🐧Linux Only)
- No need to explain.
- Cache files will released into a seperate release version.
- There will be a section in the form to upload these cache files (zipped only).
- As valve doesn't collect and combine dxvk shader caches as .foz files,these files will only include translation caches for a specific playthrough with specific cutscenes and places (if someone goes through all cutscenes and all places thats a different case).
- But still, using pre-compiled caches can reduce cpu translation overhead on low end machines.


## ⚖️ Legal Disclaimer & Notice of Non-Infringement

This repository and its maintainers are not affiliated with, authorized by, endorsed by, or in any way officially connected with Valve Corporation, Steam, or any game publishers/developers.

- **No Proprietary Game Code:** The files hosted in this repository (`.foz` / `.foz.zipped`) consist strictly of compiled SPIR-V binary pipeline states, driver state definitions, and metadata recorded by the Fossilize layer during runtime execution.
- **No Commercial Assets:** These files **do not** contain proprietary game source code, 3D models, textures, audio tracks, executable binaries (`.exe`/`.dll`), or game engine archives.
- **Fair Use & Utility:** Caches are shared strictly for interoperability, performance optimization, and archival purposes under educational and non-commercial fair use principles.

