# Minecraft 管理員手冊｜湖中暑8

## 1. 伺服器環境

```text
Ubuntu Desktop
Java 25
Paper Server 26.1.2
GeyserMC
Floodgate
playit.gg
CoreProtect
GSit
BlueMap
Cloudflare Tunnel
WorldEdit
WorldGuard
EssentialsX
EssentialsXSpawn
DecentHolograms
FancyNpcs
LuckPerms
ViaVersion
```

目前沒有安裝玩家自助領地插件，例如 GriefPrevention、Lands、Residence。

目前保護架構：

```text
出生點保護：WorldGuard + WorldEdit
玩家破壞查詢：CoreProtect
玩家自行圈地：尚未安裝
```

---

## 2. 玩家連線資訊

### Java

玩家使用 Minecraft Java Edition `26.1.2` 加入。

```text
database-unpainted.gl.joinmc.link
```

### Bedrock / 手機版

```text
governors-grown.tun.ply.gg
Port: 9794
```

---

## 3. 開服流程

開啟終端機後輸入：

```bash
sudo -u minecraft -H bash
cd /opt/minecraft/server
./start.sh
```

確認啟動成功時應看到：

```text
Started Geyser on UDP port 19132
Done!
```

---

## 4. 關服流程

請在 Minecraft 伺服器控制台輸入：

```text
stop
```

不可直接關閉終端機，避免世界資料損壞。

---

## 5. 平常建議開服方式

建議使用兩個終端機。

### 終端機 1：Minecraft 伺服器

```bash
sudo -u minecraft -H bash
cd /opt/minecraft/server
./start.sh
```

### 終端機 2：BlueMap 公開網址

```bash
cloudflared tunnel --url http://localhost:8100
```

將 Cloudflare 產生的網址分享給玩家：

```text
https://xxxxx.trycloudflare.com
```

---

## 6. BlueMap 對外分享

### 本機測試網址

```text
http://localhost:8100
```

### 確認 8100 Port 是否正在監聽

```bash
sudo ss -tulpn | grep 8100
```

正常會看到類似：

```text
tcp LISTEN *:8100
```

### 啟動 Cloudflare Quick Tunnel

```bash
cloudflared tunnel --url http://localhost:8100
```

注意：

- 不要關閉執行 `cloudflared tunnel --url http://localhost:8100` 的終端機。
- 關閉 cloudflared 終端機後，BlueMap 公開網址會失效。
- Ubuntu 重開機後，需要重新執行 cloudflared 指令。
- Cloudflare Quick Tunnel 重新建立後，網址通常會改變。

---

## 7. BlueMap 設定

設定檔：

```text
/opt/minecraft/server/plugins/BlueMap/core.conf
```

必須確認：

```text
accept-download: true
```

BlueMap 預設 Port：

```text
8100
```

---

## 8. 重要目錄

伺服器根目錄：

```text
/opt/minecraft/server
```

主世界：

```text
/opt/minecraft/server/world
```

地獄：

```text
/opt/minecraft/server/world_nether
```

終界：

```text
/opt/minecraft/server/world_the_end
```

插件：

```text
/opt/minecraft/server/plugins
```

Log：

```text
/opt/minecraft/server/logs
```

---

## 9. OP 權限

給 OP：

```text
op MuYan_TW
op .MuYan_TW
```

移除 OP：

```text
deop MuYan_TW
deop .MuYan_TW
```

建議只有管理員保留 OP，一般玩家不要給 OP。

---

## 10. 白名單管理

### 10.1 平常狀態

平常建議保持白名單開啟：

```text
whitelist on
```

查看白名單：

```text
whitelist list
```

重新讀取白名單：

```text
whitelist reload
```

### 10.2 新增 Java 玩家

```text
whitelist add 玩家名稱
```

範例：

```text
whitelist add MuYan_TW
```

### 10.3 新增基岩版玩家

基岩版玩家透過 Floodgate 進服後，名稱通常會顯示為：

```text
.玩家名稱
```

如果該玩家已經成功進入過伺服器，可以使用：

```text
whitelist add .玩家名稱
```

範例：

```text
whitelist add .MuYan_TW
```

如果基岩版玩家尚未進入過伺服器，原版 `whitelist add .玩家名稱` 可能會出現：

```text
Couldn't find profile with name
That player does not exist
```

這是因為伺服器尚未建立該 Floodgate 玩家資料。

### 10.4 使用 Floodgate 白名單

可優先嘗試：

```text
fwhitelist add 基岩版玩家Gamertag
```

注意：

```text
fwhitelist add sunny20130103
```

不要加前面的 `.`。

如果出現 Floodgate 查不到 XUID 或 cache 的錯誤，表示 Floodgate 無法透過該名稱查到玩家資料。

### 10.5 每日晚上短暫開放白名單流程

目前伺服器採用「固定時段短暫開放白名單」方式處理新玩家。

建議流程：

1. 在 Discord 公告開放時間。
2. 確認新玩家已在 Discord 或準備加入。
3. 在控制台輸入：

```text
whitelist off
```

4. 讓新玩家立即加入伺服器。
5. 查看控制台顯示的實際名稱。
6. Java 玩家加入白名單：

```text
whitelist add Java玩家名稱
```

7. 基岩版玩家加入白名單：

```text
whitelist add .基岩版玩家名稱
```

8. 加完後立刻重新開啟白名單：

```text
whitelist on
```

9. 確認名單：

```text
whitelist list
```

### 10.6 Discord 公告範例

```text
今晚白名單開放時間：20:00 - 20:10
想加入伺服器的人請在這段時間進入 Discord，並準備好 Minecraft ID。
管理員會短暫關閉白名單，等你成功進服後會立即加入白名單。
開放時間結束後會重新開啟白名單。
```

### 10.7 踢出錯誤加入玩家

如果白名單短暫關閉期間有陌生玩家加入：

```text
kick 玩家名稱
whitelist on
```

### 10.8 移除白名單玩家

```text
whitelist remove 玩家名稱
```

基岩版：

```text
whitelist remove .玩家名稱
```

---

## 11. 一人睡覺跳過夜晚

在伺服器控制台或遊戲內輸入：

```text
/gamerule playersSleepingPercentage 1
```

查看目前設定：

```text
/gamerule playersSleepingPercentage
```

恢復原版：

```text
/gamerule playersSleepingPercentage 100
```

---

## 12. 世界重生點與 EssentialsX Spawn

### 原版世界重生點

目前世界重生點：

```text
X=267
Y=69
Z=455
```

設定原版世界重生點：

```text
/setworldspawn 267 69 455
```

注意：

- Paper 26.1.2 的 `/setworldspawn` 只設定位置。
- 原版 `/setworldspawn` 不適合精準控制玩家面向方向。

### EssentialsX Spawn

站在希望玩家回到的出生點位置，面向正確方向後輸入：

```text
/setspawn
```

玩家可使用：

```text
/spawn
```

返回此出生點。

建議在：

```text
/opt/minecraft/server/plugins/Essentials/config.yml
```

確認或調整：

```yaml
respawn-at-home: true
respawn-at-home-bed: true
respawn-at-anchor: true
```

目標效果：

- 玩家有床：死亡後回床。
- 玩家有重生錨：死亡後回重生錨。
- 玩家沒有床或重生錨：死亡後回 EssentialsX 設定的 `/setspawn` 出生點。

---

## 13. EssentialsX 登入訊息

EssentialsX 登入歡迎訊息檔案：

```text
/opt/minecraft/server/plugins/Essentials/motd.txt
```

範例內容：

```text
&6歡迎來到 &b湖中暑8
```

修改後重新載入：

```text
ess reload
```

若要關閉「You have no new mail.」提示，編輯：

```text
/opt/minecraft/server/plugins/Essentials/config.yml
```

搜尋：

```yaml
notify-no-new-mail: true
```

改成：

```yaml
notify-no-new-mail: false
```

---

## 14. Geyser / Floodgate / playit 正確設定

### Bedrock 玩家連線資訊

```text
governors-grown.tun.ply.gg
Port: 9794
```

### Geyser 設定重點

設定檔：

```text
/opt/minecraft/server/plugins/Geyser-Spigot/config.yml
```

目前 playit Bedrock Tunnel 正常工作的設定是：

```yaml
bedrock:
  address: 0.0.0.0
  port: 19132
  clone-remote-port: false

java:
  auth-type: floodgate

advanced:
  bedrock:
    broadcast-port: 0
    use-haproxy-protocol: false
```

注意：

- 不要把 `use-haproxy-protocol` 改成 `true`，除非 playit Tunnel 明確啟用了 Proxy Protocol v2。
- 目前測試結果：`broadcast-port: 0` 與 `use-haproxy-protocol: false` 才能正常透過既有 playit Bedrock 網域連線。

### 檢查 Geyser 是否監聽 UDP 19132

```bash
sudo ss -lunp | grep 19132
```

正常會看到：

```text
*:19132
```

---

## 15. 原版 Spawn Protection 建議設定

因為已經使用 WorldGuard 管理出生點，建議把原版 spawn protection 關閉，避免與 WorldGuard 衝突。

編輯：

```bash
nano /opt/minecraft/server/server.properties
```

找到：

```properties
spawn-protection=16
```

改成：

```properties
spawn-protection=0
```

修改後重開伺服器。

---

## 16. WorldGuard 出生點保護區

目前已建立區域：

```text
spawn
```

目前區域範圍約為：

```text
X: 254 到 290
Y: -64 到 319
Z: 435 到 494
```

目前保護效果：

- 禁止破壞方塊
- 禁止放置方塊
- 禁止 PvP
- 禁止怪物生成
- 禁止 TNT
- 禁止苦力怕爆炸
- 禁止其他爆炸

### 查看區域資訊

```text
/rg info spawn
```

### 目前已設定的 Flags

```text
/rg flag spawn block-break deny
/rg flag spawn block-place deny
/rg flag spawn pvp deny
/rg flag spawn mob-spawning deny
/rg flag spawn creeper-explosion deny
/rg flag spawn other-explosion deny
/rg flag spawn tnt deny
```

### 建議補充設定

禁止怪物傷害玩家：

```text
/rg flag spawn mob-damage deny
```

禁止火焰擴散：

```text
/rg flag spawn fire-spread deny
```

### 加入區域擁有者

Java：

```text
/rg addowner spawn MuYan_TW
```

基岩版：

```text
/rg addowner spawn .MuYan_TW
```

---

## 17. WorldEdit / WorldGuard 常用指令

取得選區工具：

```text
//wand
```

向上、向下延伸整個世界高度：

```text
//expand vert
```

建立區域：

```text
/rg define 區域名稱
```

刪除區域：

```text
/rg remove 區域名稱
```

查看區域：

```text
/rg info 區域名稱
```

---

## 18. CoreProtect 管理指令

開啟查詢模式：

```text
/co i
```

查詢玩家最近 1 小時紀錄：

```text
/co lookup u:玩家名稱 t:1h
```

回復玩家最近 1 小時、半徑 20 格內的破壞：

```text
/co rollback u:玩家名稱 t:1h r:20
```

取消回復：

```text
/co restore u:玩家名稱 t:1h r:20
```

---

## 19. GSit 指令

```text
/sit
/lay
/crawl
/bellyflop
```

---

## 20. DecentHolograms 浮空文字

已安裝：

```text
DecentHolograms-2.10.1.jar
```

### 建立規則浮空文字

如果已建立錯誤，可以先刪除：

```text
/dh h delete rules
```

站在想顯示的位置建立：

```text
/dh h create rules &6&l伺服器規章
```

### 新增文字行

DecentHolograms 2.10.x 新增行需要指定頁面，格式為：

```text
/dh l add <hologram> <page> <content>
```

目前建議輸入：

```text
/dh l add rules 1 &f尊重、友善、公平遊玩
/dh l add rules 1 &c禁止辱罵、騷擾、歧視、洗頻
/dh l add rules 1 &c禁止外掛、作弊、利用漏洞
/dh l add rules 1 &c禁止偷竊、破壞、惡意整人
/dh l add rules 1 &c禁止破壞建築、農場、紅石機器
/dh l add rules 1 &e禁止大量生物與卡服裝置
/dh l add rules 1 &6違規將警告、停權或永久封禁
/dh l add rules 1 &b管理員保有最終處理權
/dh l add rules 1 &a祝你遊玩愉快！
```

### 常用浮空文字指令

查看所有浮空文字：

```text
/dh list
```

移動到目前位置：

```text
/dh h movehere rules
```

修改第 1 頁第 1 行：

```text
/dh l set rules 1 1 &6&l伺服器規章
```

刪除浮空文字：

```text
/dh h delete rules
```

重新載入：

```text
/dh reload
```

---

## 21. FancyNpcs NPC 管理

已安裝：

```text
FancyNpcs-2.11.0.jar
```

### 查看所有 NPC

```text
/npc list
```

### 查看 NPC 資訊

```text
/npc info NPC_ID
```

### 刪除 NPC

FancyNpcs 2.11.0 使用 `remove`，不是 `delete`。

```text
/npc remove NPC_ID
```

範例：

```text
/npc remove rules_npc
```

### 建立玩家外觀管理員 NPC

站在想放 NPC 的位置：

```text
/npc create admin_npc --type player
```

設定名稱：

```text
/npc displayname admin_npc <red><bold>湖中暑8 管理員</bold></red>
```

設定皮膚：

```text
/npc skin admin_npc MuYan_TW
```

### 設定右鍵互動訊息

```text
/npc action admin_npc RIGHT_CLICK add message <yellow>歡迎來到湖中暑8！</yellow>
/npc action admin_npc RIGHT_CLICK add message <white>請尊重、友善、公平遊玩。</white>
/npc action admin_npc RIGHT_CLICK add message <red>禁止偷竊、破壞、外掛、洗頻、惡意卡服。</red>
/npc action admin_npc RIGHT_CLICK add message <aqua>Java：database-unpainted.gl.joinmc.link</aqua>
/npc action admin_npc RIGHT_CLICK add message <aqua>Bedrock：governors-grown.tun.ply.gg Port：9794</aqua>
/npc action admin_npc RIGHT_CLICK add message <green>BlueMap 網址請向管理員索取。</green>
```

### 常用 NPC 指令

```text
/npc list
/npc info admin_npc
/npc move admin_npc
/npc fix admin_npc
/npc remove admin_npc
```

---

## 22. 領地功能狀態

目前尚未安裝玩家自助領地插件。

目前沒有以下功能：

- 玩家自己圈地
- 玩家自己保護家
- 玩家自己設定朋友權限
- 玩家自己解除領地

目前替代方案：

```text
出生點：WorldGuard 保護
破壞查詢：CoreProtect
玩家糾紛：由管理員處理
```

如果未來要加入玩家自助領地，建議考慮：

```text
GriefPrevention
Lands
Residence
```

其中朋友服最推薦先考慮 GriefPrevention。

---

## 23. 備份世界

先關服：

```text
stop
```

備份：

```bash
cd /opt/minecraft
tar -czvf backup-$(date +%F).tar.gz server/world*
```

---

## 24. 已安裝插件

```text
BlueMap
CoreProtect
DecentHolograms
Essentials
EssentialsSpawn
FancyNpcs
Floodgate
Geyser-Spigot
GSit
LuckPerms
ViaVersion
WorldEdit
WorldGuard
```

---

## 25. 常用檢查指令

查看插件：

```text
pl
```

查看 Java 版本：

```bash
java -version
```

查看世界資料夾：

```bash
ls /opt/minecraft/server/world
```

查看最新 Log：

```bash
cd /opt/minecraft/server/logs
less latest.log
```

查看 playit 狀態：

```bash
sudo systemctl status playit
```

重啟 playit：

```bash
sudo systemctl restart playit
```

---

## 26. 現行正式連線資訊（2026-08 更新）

請在公告、NPC、浮空文字與網站使用下列資訊；請勿使用舊的基岩連線資訊。

```text
🛜｜加入伺服器
JAVA版本 (26.1.2)
請在「多人遊戲」新增伺服器，輸入：
database-unpainted.gl.joinmc.link

基岩版 / 手機版
請在「遊玩 > 伺服器 > 新增伺服器」中輸入：
伺服器位址：governors-grown.tun.ply.gg
連接埠：9794
```

伺服器內部仍由 Geyser 監聽 UDP `19132`，`9794` 是提供給玩家的外部基岩連線埠。不要把 Geyser 設定中的內部監聽埠直接改成 `9794`。

---

## 27. 指令權限與安全原則

- 本服使用 **LuckPerms** 管理權限；下表的管理指令只應授權給受信任的管理員群組。
- 遊戲內輸入指令時加 `/`；在伺服器控制台輸入時不加 `/`。
- 用 `/help`、`/essentials help`、`/co help`、`//help`、`/rg help`、`/npc help` 與 `/dh help` 可查看目前版本支援的完整子指令與語法。
- 不要使用 `/reload`、`/rl` 或外掛熱重載來更新外掛。涉及 Geyser、Floodgate、Paper 與設定檔的變更，一律正常 `stop` 後重啟。
- 執行回溯、WorldEdit 大範圍操作、權限變更與白名單變更前，先確認玩家名稱、世界、範圍與時間；必要時先備份世界與資料庫。

---

## 28. 管理員完整指令索引

### 28.1 原版與 Paper 控制台管理

| 指令 | 用途 |
| --- | --- |
| `stop` | 正常儲存所有世界後關閉伺服器。 |
| `save-all`、`save-on`、`save-off` | 立即儲存或控制自動儲存；只在維護時短暫使用 `save-off`。 |
| `list` | 查看線上玩家。 |
| `say <訊息>` | 以伺服器身分公告。 |
| `kick <玩家> [原因]` | 踢出玩家。 |
| `ban <玩家> [原因]`、`pardon <玩家>` | 封鎖或解除封鎖玩家。 |
| `ban-ip <位址> [原因]`、`pardon-ip <位址>` | 封鎖或解除封鎖 IP。 |
| `op <玩家>`、`deop <玩家>` | 給予或移除 OP；一般情況建議改以 LuckPerms 群組管理。 |
| `whitelist on|off|list|reload` | 開關、查看或重新讀取白名單。 |
| `whitelist add <Java名稱>`、`whitelist remove <名稱>` | 新增或移除 Java 玩家。 |
| `fwhitelist add <基岩Gamertag>`、`fwhitelist remove <基岩Gamertag>` | 以 Floodgate 管理基岩版白名單；名稱前不要加 `.`。 |
| `gamerule <規則> <值>` | 調整世界規則，例如 `playersSleepingPercentage 1`。 |
| `setworldspawn <x> <y> <z>` | 設定原版世界重生點。 |
| `time set <day|night>`、`weather <clear|rain|thunder>` | 管理時間與天氣。 |
| `difficulty <peaceful|easy|normal|hard>` | 調整難度。 |
| `defaultgamemode <模式>`、`gamemode <模式> [玩家]` | 調整預設或個別玩家遊戲模式。 |
| `tp <目標>`、`tp <玩家> <目標>` | 傳送玩家；傳送前確認目標位置安全。 |
| `give <玩家> <物品> [數量]`、`clear <玩家> [物品]` | 給予或清除物品。 |
| `effect give|clear`、`enchant`、`experience` | 管理效果、附魔與經驗值。 |

### 28.2 EssentialsX 與出生點管理

| 指令 | 用途 |
| --- | --- |
| `/essentials`、`/ess reload` | 查看 EssentialsX 資訊；`/ess reload` 僅適合小幅設定調整，避免用於全服／外掛更新。 |
| `/setspawn [群組]`、`/spawn [玩家]` | 設定或傳送至 EssentialsX 出生點。 |
| `/setwarp <名稱>`、`/delwarp <名稱>`、`/warp <名稱>`、`/warps` | 建立、刪除、使用與查看公開傳送點。 |
| `/sethome [玩家] [名稱]`、`/delhome [玩家] [名稱]`、`/home [玩家] [名稱]` | 管理自己或指定玩家的家。 |
| `/tp <玩家>`、`/tphere <玩家>`、`/tpall`、`/tppos <x> <y> <z>` | EssentialsX 傳送工具。 |
| `/back [玩家]` | 讓玩家返回傳送前位置或死亡點。 |
| `/heal [玩家]`、`/feed [玩家]`、`/fly [玩家]`、`/god [玩家]` | 生命、飽食、飛行與無敵管理。 |
| `/invsee <玩家>`、`/enderchest [玩家]`、`/clearinventory [玩家]` | 檢視或管理背包與終界箱；操作前需確認事由。 |
| `/seen <玩家>`、`/whois <玩家>`、`/realname <名稱>` | 查詢玩家基本資料。 |
| `/mute <玩家> [時間] [原因]`、`/unmute <玩家>`、`/jail`、`/unjail` | 管理處分（僅在已配置對應功能時使用）。 |
| `/broadcast <訊息>`、`/mail send <玩家> <訊息>`、`/socialspy` | 公告、寄信與社交監看。 |
| `/nick <玩家> <名稱>`、`/itemname`、`/setworth`、`/worth` | 暱稱、物品名稱與物品價值管理。 |

### 28.3 LuckPerms 權限管理

| 指令 | 用途 |
| --- | --- |
| `/lp`、`/lp help` | LuckPerms 主指令與說明。 |
| `/lp user <玩家> info` | 查看玩家目前群組與權限。 |
| `/lp user <玩家> parent add <群組>` | 將玩家加入群組。 |
| `/lp user <玩家> parent remove <群組>` | 從群組移除玩家。 |
| `/lp user <玩家> permission set <節點> true|false` | 為個別玩家授權或拒絕權限。 |
| `/lp group <群組> permission set <節點> true|false` | 管理群組權限。 |
| `/lp group <群組> parent add <上層群組>` | 設定群組繼承。 |
| `/lp editor` | 開啟網頁權限編輯器；套用前再次檢查差異。 |
| `/lp sync`、`/lp save` | 與外部儲存同步或儲存；目前使用本機 H2 儲存時通常不需手動同步。 |

### 28.4 CoreProtect 稽核與回復

| 指令 | 用途 |
| --- | --- |
| `/co inspect`、`/co i` | 開啟／關閉點擊方塊查詢模式。 |
| `/co lookup [參數]`、`/co l [參數]` | 查詢紀錄。常用：`/co l u:玩家 t:1h r:20`。 |
| `/co rollback [參數]` | 回復符合篩選的變更。範例：`/co rollback u:玩家 t:1h r:20`。 |
| `/co restore [參數]` | 撤銷先前回復；篩選條件必須與回復一致。 |
| `/co near` | 查詢附近紀錄。 |
| `/co status` | 查看 CoreProtect 佇列與狀態。 |
| `/co purge t:<時間>` | 清除過舊紀錄；執行前先備份 `plugins/CoreProtect/database.db`。 |

CoreProtect 篩選常用縮寫：`u:` 玩家、`t:` 時間、`r:` 半徑、`w:` 世界、`a:` 動作、`b:` 方塊／物品。先用 `lookup` 驗證結果，再使用 `rollback`。

### 28.5 WorldEdit 建設指令

| 指令 | 用途 |
| --- | --- |
| `//wand` | 取得選取工具。 |
| `//pos1`、`//pos2` | 設定第一與第二個選取點。 |
| `//hpos1`、`//hpos2` | 將看向的方塊設為選取點。 |
| `//expand <數量> <方向>`、`//expand vert` | 延伸選區；建立 WorldGuard 區域前常用 `//expand vert`。 |
| `//contract <數量> <方向>`、`//shift <數量> <方向>` | 縮小或位移選區。 |
| `//set <方塊>`、`//replace <來源> <目標>`、`//overlay <方塊>` | 填滿、替換或覆蓋選區方塊。 |
| `//copy`、`//cut`、`//paste [-a]` | 複製、剪下與貼上建築。 |
| `//rotate <角度>`、`//flip [方向]` | 旋轉或翻轉剪貼簿。 |
| `//undo [次數]`、`//redo [次數]` | 復原或重做 WorldEdit 操作。 |
| `//schem save <名稱>`、`//schem load <名稱>`、`//schem list` | 儲存、載入與列出建築藍圖。 |
| `//regen` | 重新生成選區；高風險操作，請先備份。 |

### 28.6 WorldGuard 區域保護

| 指令 | 用途 |
| --- | --- |
| `/rg define <區域>`、`/rg remove <區域>` | 以目前 WorldEdit 選區建立或移除區域。 |
| `/rg info [區域]`、`/rg list [玩家]` | 查看區域資訊或玩家所在／擁有的區域。 |
| `/rg flag <區域> <旗標> <值>` | 設定旗標。例：`/rg flag spawn pvp deny`。 |
| `/rg addowner <區域> <玩家>`、`/rg removeowner <區域> <玩家>` | 管理區域擁有者。 |
| `/rg addmember <區域> <玩家>`、`/rg removemember <區域> <玩家>` | 管理區域成員。 |
| `/rg setpriority <區域> <數值>` | 設定重疊區域優先順序。 |
| `/rg setparent <子區域> <父區域>` | 設定區域父子關係。 |
| `/rg teleport <區域>` | 傳送至區域（需對應權限）。 |

常用保護旗標：`block-break`、`block-place`、`pvp`、`mob-spawning`、`mob-damage`、`tnt`、`creeper-explosion`、`other-explosion`、`fire-spread`、`chest-access`。

### 28.7 NPC、全像投影與地圖

| 外掛 | 指令 | 用途 |
| --- | --- | --- |
| FancyNpcs | `/npc list`、`/npc info <ID>`、`/npc create <ID> --type player` | 列表、資訊與建立 NPC。 |
| FancyNpcs | `/npc displayname <ID> <文字>`、`/npc skin <ID> <玩家>`、`/npc move <ID>`、`/npc remove <ID>` | 修改名稱、皮膚、位置與刪除 NPC。 |
| FancyNpcs | `/npc action <ID> RIGHT_CLICK add message <訊息>` | 加入右鍵顯示訊息互動。 |
| DecentHolograms | `/dh list`、`/dh h create <名稱> <文字>`、`/dh h delete <名稱>` | 列出、建立、刪除全像投影。 |
| DecentHolograms | `/dh l add <投影> <頁> <文字>`、`/dh l set <投影> <頁> <行> <文字>` | 新增或修改文字行。 |
| DecentHolograms | `/dh h movehere <名稱>`、`/dh reload` | 移到目前位置或重新讀取設定。 |
| BlueMap | `/bluemap`、`/bluemap help` | 查看 BlueMap 可用管理子指令；渲染與網站設定請在低峰時段操作。 |

### 28.8 Geyser、Floodgate 與 ViaVersion

| 指令 | 用途 |
| --- | --- |
| `/geyser help`、`/geyser version` | 查看 Geyser 說明與目前版本。 |
| `/geyser dump` | 產生診斷資料；分享前檢查是否含敏感資訊。 |
| `/fwhitelist add|remove <Gamertag>`、`/fwhitelist list` | 管理基岩版白名單。 |
| `/linkaccount`、`/unlinkaccount` | Floodgate 帳號連結／解除連結（依設定與權限可用）。 |
| `/viaversion list`、`/viaversion pps`、`/viaversion dump` | 檢查 Java 用戶端版本、封包速率與診斷資料。 |

### 28.9 GSit 管理

| 指令 | 用途 |
| --- | --- |
| `/gsitreload`、`/gsitrl` | 重新讀取 GSit 設定。 |
| `/gsit kick <玩家>` | 讓玩家離開坐姿／姿勢；僅在需要處理卡住時使用。 |
| `/sit`、`/lay`、`/bellyflop`、`/crawl`、`/spin` | 可用於測試玩家姿勢功能。 |

---

## 29. Paper 紅石複製相容設定

TNT、地毯與鐵軌等活塞複製機由 Paper 核心設定控制，**不是外掛擋住**。設定檔為：

```text
/opt/minecraft/server/config/paper-global.yml
```

應維持：

```yaml
unsupported-settings:
  allow-piston-duplication: true
```

修改後必須正常重啟才會生效。請監看大量 TNT 複製機與同時爆炸，必要時要求玩家分批運作，以避免 TPS 明顯下降。
