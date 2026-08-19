# 湖中暑 8｜Minecraft 伺服器

這是一個以 Paper 為核心的生存伺服器，提供 Java 版與基岩版跨平台遊玩。伺服器使用原版世界機制，並透過外掛提供傳送、權限、保護、NPC、地圖與基岩版連線支援。

## 加入伺服器

### Java 版（26.1.2）

請在「多人遊戲」新增伺服器，輸入：

```text
database-unpainted.gl.joinmc.link
```

### 基岩版／手機版

請在「遊玩 > 伺服器 > 新增伺服器」中輸入：

```text
伺服器位址：governors-grown.tun.ply.gg
連接埠：9794
```

## 目前版本與平台

- 伺服器核心：Paper `26.1.2`（build 74）
- Java 執行環境：OpenJDK 25
- Java 伺服器連接埠：`25565/TCP`
- 基岩伺服器連接埠：`19132/UDP`
- 基岩版橋接：Geyser `2.11.1` + Floodgate `2.2.5`（build 140）
- 自訂資源包：目前未設定；加入伺服器不需要下載額外資源包。
- 自訂資料包：目前未安裝額外資料包。

## 可以使用的功能

### 跨平台遊玩

- **Geyser + Floodgate**：基岩版玩家可直接加入，不需要 Java Edition 帳號。
- 基岩版玩家在伺服器內會以 `.` 作為名稱前綴，例如 `.PlayerName`。
- **ViaVersion**：讓較新版的 Java 客戶端能連線至目前伺服器版本。

### 生存與便利功能

- **EssentialsX**：提供常用的伺服器指令與便利功能，例如重生點、家、傳送請求、私訊、傳送、套件與管理指令；可用指令依玩家權限而定。
- **EssentialsXSpawn**：控制玩家首次登入與使用 `/spawn` 時的重生點行為。
- **GSit**：可使用坐下、躺下、爬行與姿勢相關功能；常用指令包含 `/sit`、`/lay`、`/crawl`。
- **自己的玩家頭顱**：輸入 `/kit head`，即可取得一顆使用自己目前玩家造型的頭顱，不必輸入玩家名稱。

### 世界建設與展示

- **WorldEdit**：供有建設權限的管理者快速選取、複製、貼上與編輯區域。
- **WorldGuard**：保護特定區域並設定建築、互動、PVP 等規則。
- **DecentHolograms**：建立文字、物品或展示用的全像投影。
- **FancyNpcs**：建立具外觀、對話與互動行為的 NPC。
- **BlueMap**：產生可在瀏覽器查看的 3D 世界地圖；地圖網址由管理員另行提供。

### 權限、稽核與保護

- **LuckPerms**：管理玩家群組與外掛指令權限。
- **CoreProtect**：記錄方塊、容器與玩家活動，供管理員查詢、回溯或還原破壞。

## 已安裝外掛

| 外掛 | 目前版本 | 用途 |
| --- | --- | --- |
| BlueMap | 5.22 | 3D 世界地圖 |
| CoreProtect | 24.0 | 行為記錄與回溯保護 |
| DecentHolograms | 2.10.1 | 全像投影 |
| EssentialsX | 2.22.0 | 常用指令與伺服器功能 |
| EssentialsXSpawn | 2.22.0 | 重生點控制 |
| FancyNpcs | 2.11.0 | NPC |
| Floodgate | 2.2.5 build 140 | 基岩帳號登入與皮膚支援 |
| Geyser | 2.11.1 build 1219 | Java／基岩協定橋接 |
| GSit | 3.5.1 | 坐、躺、爬行與姿勢 |
| LuckPerms | 5.5.59 | 權限管理 |
| ViaVersion | 5.11.0 | Java 用戶端版本相容 |
| WorldEdit | 7.4.4 | 世界編輯 |
| WorldGuard | 7.0.16 | 區域保護 |

## 紅石與複製機

伺服器已啟用 Paper 的活塞複製相容設定，因此 TNT、地毯與鐵軌等依賴活塞複製機制的紅石機器可以使用。大量 TNT 複製或同時引爆仍可能造成伺服器延遲，請避免在玩家密集或重要建築附近長時間大量運作。

## 管理提醒

- 不要使用 `/reload` 重新載入外掛；更新外掛或 Paper 設定後，請正常停止並重啟伺服器。
- 管理、WorldEdit、WorldGuard、CoreProtect 與部分 EssentialsX 功能需要由 LuckPerms 授權。
- 如基岩版無法加入，請確認外部位址為 `governors-grown.tun.ply.gg`、連接埠為 `9794`；伺服器內部的 Geyser 仍使用 UDP `19132`。
