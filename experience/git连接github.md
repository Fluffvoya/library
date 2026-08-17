**核心思路**：通过密钥对认证，配置一次后永久免密。

**关键步骤**：

1. **生成密钥**：`ssh-keygen -t ed25519 -C "你的邮箱"`
    
2. **添加公钥**：将 `~/.ssh/id_ed25519.pub` 内容粘贴到 GitHub → Settings → SSH and GPG keys。
    
3. **改远程地址**：`git remote set-url origin git@github.com:用户名/仓库名.git`
    
4. **验证连接**：`ssh -T git@github.com`，看到成功提示即完成。
    

> ⚠️ 私钥（`id_ed25519`）务必保密，切勿泄露。