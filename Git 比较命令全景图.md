### Git 比较命令全景图

```
                  git add             git commit            git push
      ┌──────────┐  ➜   ┌──────────┐  ➜   ┌──────────┐  ➜   ┌──────────────┐
      │  工作区   │       │  暂存区   │      │  本地库   │      │ 远程跟踪分支  │
      │Workspace │       │  Index   │      │Local Repo│      │Remote/Origin │
      └────┬─────┘       └────┬─────┘      └────┬─────┘      └──────┬───────┘
           │                  │                 │                   │
           │                  │                 │                   │
①   ──────>│<── (COMMAND) ──> │                 │                   │
           │ git difftool     │                 │                   │
           │   --dir-diff     │                 │                   │
           │                  │                 │                   │
②  ──────>│                 │<── (COMMAND) ──>│                   │
           │                 │  git difftool   │                   │
           │                 │   --dir-diff    │                   │
           │                 │    --cached     │                   │
           │                 │                 │                   │
③  ──────>│<────────── (COMMAND) ────────────>│                   │
           │       git difftool --dir-diff     │                   │
           │                HEAD               │                   │
           │                                   │                   │
④  ──────>│                                   │<── (COMMAND) ──>│
           │                                   │   git difftool    │
           │                                   │    --dir-diff     │
           │                                   │ HEAD origin/master│
           │                                   │                   │
⑤  ──────>│<──────────────────── (COMMAND) ──────────────────────>│
                  git difftool --dir-diff origin/master
```

------

### 图解说明

1. **① 工作区 ↔ 暂存区**
   - **命令：** `git difftool --dir-diff`
   - **场景：** 最常用。写了一半代码，想看看改了啥，还没 `add`。
2. **② 暂存区 ↔ 本地库**
   - **命令：** `git difftool --dir-diff --cached`
   - **场景：** 已经 `add` 了，在 `commit` 之前做最后检查。
3. **③ 工作区 ↔ 本地库 (HEAD)**
   - **命令：** `git difftool --dir-diff HEAD`
   - **场景：** 不管有没有 `add`，想看当前文件和最近一次提交相比的总变化。
4. **④ 本地库 (HEAD) ↔ 远程分支**
   - **命令：** `git difftool --dir-diff HEAD origin/master`
   - **场景：** 本地已经 `commit` 了，准备 `push` 之前，检查一下和服务器上的代码有什么不同（防止冲突或错误提交）。
5. **⑤ 工作区 ↔ 远程分支**
   - **命令：** `git difftool --dir-diff origin/master`
   - **场景：** 这几天写了很多代码（还没提交），想看看和服务器上最新的代码差距有多大。

**建议：** 如果是为了日常开发，**死记 ① 和 ③** 就足够应付绝大多数情况了。



