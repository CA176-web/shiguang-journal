# Google Drive 首次配置

适用于本分享包的拾光日记 0.9.1 Windows x64 版。

0.7.0 增加客户端配置、账号、口令及操作状态检查，分别提示授权拒绝与账号策略限制。另可在“设置 → 服务 → 从本机 Diarium 导入”读取正版已同步到电脑的数据；这是单向导入，与本页的拾光云同步分别使用。

拾光可以通过浏览器登录你的 Google 账号，在多台拾光 Windows 客户端之间加密同步。当前没有随程序提供可直接使用的公共 Google OAuth 客户端，需要先为自己注册一个“桌面应用”客户端并导入 JSON。没有这份有效配置时，登录不会成功；这不影响本地写日记和加密备份。

**此功能不能与安卓 Diarium 的私有云数据互通。** 拾光使用独立数据格式和 Google Drive 隐藏应用数据区域；登录相同 Google 账号也不会读取 Diarium 的 `Diarium` 文件夹。原版的云同步入口和自动同步流程见 [Diarium 官方帮助](https://diariumapp.com/help)，原版文件位置见 [官方同步说明](https://forum.diariumapp.com/d/13-sync)。

## 1. 在 Google Cloud 建立个人客户端

1. 打开 [Google Cloud 控制台](https://console.cloud.google.com/)，选择或建立一个你自己管理的项目。记住该项目，后续步骤都在这个项目中完成。
2. 进入“API 和服务 → 库”，搜索 **Google Drive API** 并启用。需要启用的是 Drive API，不是 Google Calendar API。[官方 API 启用说明](https://developers.google.com/identity/protocols/oauth2/native-app#enable-apis)
3. 进入“Google Auth platform → Branding（品牌）”。首次设置时点击 Get Started，填写应用名（例如“我的拾光日记”）、支持邮箱和联系邮箱。个人 Google 账号通常选择 **External（外部）**；Workspace 的 Internal 类型只适用于符合条件的组织内用户。
4. 在“Audience（受众）”里，如果应用处于 **Testing（测试）** 状态，将准备登录的 Google 邮箱加入 **Test users（测试用户）**。仅创建客户端而没有加入测试用户，可能在登录时被拒绝。
5. 在“Data Access（数据访问）”中声明本程序使用的权限：`https://www.googleapis.com/auth/drive.appdata`、`openid`、邮箱权限 `email`（控制台可能显示为 `https://www.googleapis.com/auth/userinfo.email`）。不需要申请读取整个 Google Drive、Gmail 或日历的权限。界面名称可能随 Google 更新；按 [官方同意屏幕配置说明](https://developers.google.com/workspace/guides/configure-oauth-consent) 核对。
6. 进入“Google Auth platform → Clients（客户端）→ Create Client”，应用类型选 **Desktop app（桌面应用）**，填写名称并创建。下载该客户端的 JSON 文件；文件内应有 `installed` 对象和 `.apps.googleusercontent.com` 结尾的 `client_id`。不要选 Web application、Android 或服务账号，也不要用 API key 代替。[官方桌面 OAuth 说明](https://developers.google.com/identity/protocols/oauth2/native-app)

不需要为拾光搭建公网回调网站。登录使用系统浏览器，回调到本机临时的 `127.0.0.1` 端口；程序使用 PKCE 和随机 state 校验这次登录。请保留原始 JSON，后续其他拾光电脑需要导入同一份配置；不要把 Google 密码、授权码或刷新令牌发到聊天或写进公开文档。

## 2. 在拾光登录并完成首次同步

1. 先通过“关于 → 创建备份”建立一份独立加密备份。
2. 点击左侧底部同步图标，或“设置 → 服务 → 云同步 · Google Drive / WebDAV…”。选择 **Google Drive**。
3. 展开“Google 登录配置”，导入刚下载的桌面客户端 JSON。配置会加密保存在本机日记库。
4. 点击 **登录 Google 账号**，在系统浏览器中选择预期账号，核对你自己的应用名称并授予所需权限。Google 密码只在 Google 页面输入。浏览器提示已收到授权后，回到拾光查看是否显示正确邮箱；浏览器页面本身不代表同步完成。
5. 输入一份 **8–1024 个字符的独立同步口令**。它不是 Google 密码，也不必等于本地主密码或备份密码；所有参与同步的拾光电脑必须使用同一口令。已有云端数据时不能直接改口令完成换钥。
6. 可先点“检查连接”验证读取权限和已有云库口令；它不代替首次写入验证。点击 **立即同步**，等待窗口显示同步结果，再核对日记和附件。
7. 首次同步成功后，如需定时同步，勾选“程序打开并解锁时自动同步”，设置 **5–1440 分钟**并保存。程序关闭、锁定或休眠时不会作为系统服务继续同步；编辑中、输入法组合尚未确认或窗口状态变化时会延后。

登录成功本身不会立即上传整个日记库。换账号、重新导入客户端配置或退出后，先核对账号和本机内容，再手动同步；不要仅依靠原来的自动开关。

## 3. 第二台电脑

在另一台电脑运行拾光，导入 **同一份桌面客户端 JSON**，登录 **同一个 Google 账号**，使用 **相同的同步口令**，然后点击“立即同步”。本实现使用客户端 ID 和 Google 账号稳定标识划分同步库；新建另一个客户端 ID 会进入另一个拾光同步库，即使邮箱相同。重新安装时也应保留原客户端配置和同步口令。

新电脑已有日记时，同步会合并两边内容，出现并发修改会保留“同步冲突副本”。切换账号不会清空本机日记；随后首次同步会将当前本机日记与新账号的拾光云库合并。Google Drive 和 WebDAV 各有独立同步状态，切换提供商不会自动搬移另一提供商的云端数据。

## 4. 数据位置、权限和退出

日记正文、元数据、附件和回收站状态在上传前使用同步口令加密。本机外观、服务授权、目录配置、当天动态和版本历史不参与云同步。Google 登录凭据加密保存在本机日记库；日记云同步不会上传这些凭据。手动或自动创建的完整 `.journalbackup` 备份会包含设置中的凭据，整份备份另用备份密码加密；恢复时保留目标电脑的 Google 配置，不激活来源电脑的账号。

云端位于 Google Drive 的 **appDataFolder 隐藏应用数据区域**，普通“我的云端硬盘”页面不会显示一个拾光文件夹。该权限只用于此应用的私有数据，不允许读取你的全部云盘文件；用户仍可能在 Google 的应用管理中删除应用数据。[Google 隐藏应用数据说明](https://developers.google.com/workspace/drive/api/guides/appdata)

点击“退出账号”只清除此电脑的授权并关闭自动同步，保留本机日记、云端数据和其他电脑的登录。它不调用全项目撤销授权。若你在 Google 账号安全设置中撤销应用访问权限，其他使用这个项目的设备也可能需要重新登录；Google 的撤销机制见 [官方授权撤销说明](https://developers.google.com/identity/protocols/oauth2/web-server#tokenrevoke)。

同步会传播删除，不能替代备份。目前永久删除墓碑不会过期，旧提交和不再引用的密文附件不自动清理。每个附件最多 200 MB，每轮附件传输最多 512 MB；单次加密变更最多 32 MB，总历史最多 256 MB、20,000 次提交，最多 100,000 条记录（含删除标记）。达到限额时程序会停止并提示；本版没有自动历史压缩或云端垃圾回收。

取消或断网时，已经被 Google 成功接收的加密对象可能保留；下次同步重新核对，不承诺撤回已提交的远端内容。云端已确认记录被删或改写时会停止同步，避免把异常空库直接覆盖到本机。

## 5. 常见问题

| 现象 | 处理 |
| --- | --- |
| 提示尚未配置桌面客户端 | 先完成第 1 节并导入 JSON，程序没有内置可替代的公共客户端。 |
| JSON 无效或类型不适用 | 重新下载 Desktop app 客户端 JSON；不要导入服务账号密钥、Web 客户端或 API key。 |
| Google 提示 access_denied／账号不在测试用户中 | 在同一 Cloud 项目的 Audience 中加入该邮箱，或检查组织管理员的应用访问限制。 |
| 登录约三分钟后超时 | 确认默认浏览器可打开 Google，完成授权后能回到本机回调；检查代理/安全软件对 loopback 的影响，再重新发起登录。不要把授权码手动贴入程序。 |
| “Drive 拒绝访问” | 确认当前项目启用了 Drive API、同意应用数据权限，并检查 Google 存储空间、API 配额和组织策略。 |
| 几天后要求重新登录 | External + Testing 且申请 Drive 权限时，刷新令牌通常 **7 天后到期**。这是 Google 的测试模式规则；本程序暂停自动同步并提示重新登录，不会在后台反复弹浏览器。[令牌过期说明](https://developers.google.com/identity/protocols/oauth2/#expiration) |
| 想长期使用而不每周登录 | 可按 Google 控制台流程评估发布为 Production；是否需要验证、允许哪些用户由项目、权限和 Google 政策决定。发布不保证令牌永久有效，用户撤销等情形仍需重新登录。不要仅为跳过提示随意扩大权限。 |
| 另一电脑看不到日记 | 核对同一客户端 ID、同一 Google 账号和同一同步口令；先让有日记的一台完成同步，再同步另一台。登录原版安卓 Diarium 不会加入此同步库。 |
| 同步口令错误／云端数据损坏 | 保留本机日记，核对原口令；不要删除隐藏数据来尝试修复。独立备份可用于恢复到新日记库。 |

云端同步需要有效的 Google 授权和可用的网络连接。账号或组织限制、存储空间和配额都可能影响同步。
