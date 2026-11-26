# openmini-vault

小盒子「私有市场」的**密文镜像**。

这里只有加密文件，没有任何明文、没有密钥、没有口令：

- `meta.json` — 公开的 KDF 参数（盐、迭代次数）
- `catalog.enc` — 加密的应用清单
- `apps/<hash>.enc` — 加密的应用包

文件格式：`"OMV1" | nonce(12B) | AES-256-GCM 密文`，AAD 绑定文件路径。

没有口令派生的密钥，这些文件无法解密，也不构成任何可用工具。授权用户在小盒子宿主里输入账号口令后，
宿主在本机解密、校验 sha256 并安装；仓库本身不参与鉴权。

真正的私有源在 `joeyjiaojg/openmini-market-private`（私有仓库）。本仓库只是它的密文镜像，
方便没有在宿主编译期配置只读 token 的设备也能拉取密文。宿主支持直接读私有仓：在 `Vault.PRIVATE_TOKEN`
填上只读 fine-grained PAT 重新打包即可，那时本镜像可以整个删掉。
