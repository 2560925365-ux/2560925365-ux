# GitHub README API 使用指南

## 1. 动态统计图 API 调用代码

### GitHub Readme Stats

**基础统计卡片：**
```markdown
![GitHub Stats](https://github-readme-stats.vercel.app/api?username=2560925365-ux&show_icons=true&theme=tokyonight&hide_border=true&hide_rank=true&card_width=500)
```

**参数说明：**
| 参数 | 值 | 说明 |
|------|-----|------|
| username | 2560925365-ux | GitHub 用户名 |
| show_icons | true | 显示图标 |
| theme | tokyonight | 主题（default/dimply/dracula/tokyonight等） |
| hide_border | true | 隐藏边框 |
| hide_rank | true | 隐藏排名 |
| card_width | 500 | 卡片宽度 |

**语言统计卡片：**
```markdown
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=2560925365-ux&layout=compact&theme=tokyonight&hide_border=true&hide_title=true&card_width=500)
```

**完整 API 文档：** https://github.com/anuraghazra/github-readme-stats

---

## 2. 项目信息获取的 Python 脚本

```python
#!/usr/bin/env python3
"""
获取 GitHub 仓库信息并生成 README 项目卡片
"""

import requests
from typing import List, Dict

# GitHub API 配置
GITHUB_API = "https://api.github.com"
USERNAME = "2560925365-ux"
REPOS = ["french-tutor", "Draftcat", "hydra-ocr"]

def get_repo_info(owner: str, repo: str) -> Dict:
    """获取单个仓库信息"""
    url = f"{GITHUB_API}/repos/{owner}/{repo}"
    response = requests.get(url)
    response.raise_for_status()
    data = response.json()

    return {
        "name": data["name"],
        "description": data.get("description", "No description"),
        "stars": data["stargazers_count"],
        "forks": data["forks_count"],
        "language": data.get("language", "Unknown"),
        "url": data["html_url"]
    }

def get_all_repos(owner: str, repos: List[str]) -> List[Dict]:
    """获取所有仓库信息"""
    return [get_repo_info(owner, repo) for repo in repos]

def generate_project_card(repo: Dict) -> str:
    """生成项目卡片 HTML"""
    return f'''
<a href="{repo['url']}">
  <div style="border: 1px solid #30363d; border-radius: 8px; padding: 16px; background: #0d1117;">
    <h3 style="margin: 0 0 8px 0;">{repo['name']}</h3>
    <p style="color: #8b949e; margin: 0 0 12px 0; font-size: 14px;">{repo['description']}</p>
    <div style="display: flex; gap: 12px; align-items: center;">
      <img src="https://img.shields.io/badge/{repo['language']}-3776AB?style=flat-square" alt="{repo['language']}" />
      <img src="https://img.shields.io/github/stars/{USERNAME}/{repo['name']}?style=social" alt="Stars" />
      <img src="https://img.shields.io/github/forks/{USERNAME}/{repo['name']}?style=social" alt="Forks" />
    </div>
  </div>
</a>'''

def generate_readme():
    """生成 README.md"""
    repos = get_all_repos(USERNAME, REPOS)

    print("## 🚀 Projects\n")
    print("<table><tr>")

    for i, repo in enumerate(repos):
        if i > 0 and i % 2 == 0:
            print("</tr><tr>")
        print(f"<td width='50%'>{generate_project_card(repo)}</td>")

    print("</tr></table>")

if __name__ == "__main__":
    generate_readme()
```

---

## 3. 技术栈徽章的 Markdown 嵌入代码

### Shields.io 基础徽章

**语法格式：**
```
https://img.shields.io/badge/<LABEL>-<MESSAGE>-<COLOR>
```

**常用技术徽章：**

| 技术 | 徽章代码 |
|------|---------|
| Python | `![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)` |
| JavaScript | `![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat-square&logo=javascript&logoColor=black)` |
| HTML5 | `![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)` |
| CSS3 | `![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)` |
| FastAPI | `![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)` |

**GitHub 动态徽章：**

| 类型 | 代码 |
|------|------|
| Stars | `![Stars](https://img.shields.io/github/stars/2560925365-ux/french-tutor?style=social)` |
| Forks | `![Forks](https://img.shields.io/github/forks/2560925365-ux/french-tutor?style=social)` |
| Issues | `![Issues](https://img.shields.io/github/issues/2560925365-ux/french-tutor)` |

**样式参数：**
- `style=flat` - 扁平样式
- `style=flat-square` - 扁平方形（推荐）
- `style=plastic` - 塑料效果
- `style=social` - 社交样式（用于 GitHub stars/forks）

**完整 API 文档：** https://shields.io

---

## 4. HTML/CSS 卡片样式说明

### 暗色主题配色

```css
/* GitHub 暗色主题配色变量 */
--bg-primary: #0d1117;
--bg-secondary: #161b22;
--border-color: #30363d;
--text-primary: #c9d1d9;
--text-secondary: #8b949e;
--accent-color: #58a6ff;
```

### 响应式布局

```html
<!-- 表格布局实现响应式 -->
<table>
<tr>
<td width="50%">内容1</td>
<td width="50%">内容2</td>
</tr>
</table>
```

### 卡片组件样式

```css
.project-card {
  border: 1px solid #30363d;
  border-radius: 8px;
  padding: 16px;
  background: #0d1117;
  transition: transform 0.2s, box-shadow 0.2s;
}

.project-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}
```
