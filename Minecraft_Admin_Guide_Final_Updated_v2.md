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

```text
database-unpainted.gl.joinmc.link
```

### Bedrock / 手機版

```text
traffic-convert.gl.at.ply.gg
Port: 57476
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
traffic-convert.gl.at.ply.gg
Port: 57476
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
/npc action admin_npc RIGHT_CLICK add message <aqua>Bedrock：traffic-convert.gl.at.ply.gg Port：57476</aqua>
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
