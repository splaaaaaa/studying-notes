# React 项目中使用 Git 分支开发组件的流程

## 一、创建新分支

基于主分支（如 `main` 或 `master`）创建并切换到开发分支，用于独立开发新组件：

```bash
# 创建并切换到 feature-component 分支（命名建议：feature-组件名）
git checkout -b feature-component
```

## 二、开发组件并提交更改

在新分支中开发 React 组件，通过 Git 记录变更：

1. **开发组件**：
   在 `src/components/` 等目录下编写组件代码（如 `NewComponent.jsx`），并在需要的地方引入使用。

2. **暂存并提交更改**：

   ```bash
   # 将所有修改添加到暂存区
   git add .
   
   # 提交更改（描述需清晰，如“添加 NewComponent 组件”）
   git commit -m "Add NewComponent with basic functionality"
   ```

   （可选：开发过程中可多次提交，逐步记录进度）

## 三、推送分支到远程仓库

将本地开发分支同步到远程仓库，便于协作或备份：

```bash
# 推送本地分支到远程（首次推送需指定关联关系）
git push origin feature-component
```

远程仓库会自动创建同名分支（如 `feature-component`）。

## 四、合并到主分支

开发完成并测试无误后，将新组件整合到主分支：

1. **切换到主分支**：

   ```bash
   git checkout main
   ```

2. **拉取远程主分支最新代码**（避免冲突）：

   ```bash
   git pull origin main
   ```

3. **合并开发分支到主分支**：

   ```bash
   git merge feature-component
   ```

   - 若出现**合并冲突**：需手动编辑冲突文件（找到 `<<<<<<< HEAD` 标记的冲突部分），解决后执行 `git add .` 和 `git commit -m "Resolve merge conflicts"` 完成合并。

## 五、推送主分支更新

将合并后的主分支推送到远程，完成最终同步：

```bash
git push origin main
```

## 六、（可选）删除分支

若开发分支不再需要，可清理本地和远程分支：

```bash
# 删除本地分支
git branch -d feature-component

# 删除远程分支
git push origin --delete feature-component
```

## 总结流程

1. 建分支 → 2. 开发提交 → 3. 推远程 → 4. 合主分支 → 5. 清分支
   通过分支隔离开发，确保主分支代码稳定性，同时便于多人协作和版本管理。