# FitCheck — Deploy as Phone App

## 最快方法：GitHub Pages + 手机添加到主屏幕（5分钟）

### Step 1: 上传到 GitHub Pages

1. 去 https://github.com → 点 "New repository"
2. 名字写 `fitcheck`，选 Public，点 Create
3. 点 "uploading an existing file"
4. 把这个文件夹里的所有文件拖进去：
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - `icon-192.svg`
   - `icon-512.svg`
5. 点 "Commit changes"
6. 去 Settings → Pages → Source 选 "main" branch → Save
7. 等1分钟，你的网址就是：`https://你的用户名.github.io/fitcheck/`

### Step 2: 解决 API CORS（让 AI 真正工作）

在 Cloudflare Workers 创建一个代理：

1. 去 https://workers.cloudflare.com → 免费注册
2. 点 "Create a Worker"
3. 把代码替换成：

```javascript
export default {
  async fetch(request) {
    if (request.method === 'OPTIONS') {
      return new Response(null, {
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Methods': 'POST',
          'Access-Control-Allow-Headers': 'Content-Type',
        }
      });
    }
    const body = await request.json();
    const resp = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'x-api-key': 'YOUR_ANTHROPIC_API_KEY',   // ← 替换这里
        'anthropic-version': '2023-06-01',
      },
      body: JSON.stringify(body),
    });
    const data = await resp.json();
    return new Response(JSON.stringify(data), {
      headers: { 'Access-Control-Allow-Origin': '*', 'Content-Type': 'application/json' }
    });
  }
};
```

4. 保存，会得到一个 URL 比如 `https://fitcheck-proxy.你的名字.workers.dev`
5. 在 `index.html` 里搜索 `api.anthropic.com/v1/messages`，
   替换为你的 Worker URL（共2处）

### Step 3: 手机安装为 App

**iPhone (Safari)**：
1. Safari 打开你的 GitHub Pages 网址
2. 点底部分享按钮 →「添加到主屏幕」
3. 名字写 FitCheck → 添加
4. 主屏幕出现图标，点开就是全屏 App！

**Android (Chrome)**：
1. Chrome 打开网址
2. 右上角菜单 →「添加到主屏幕」或底部会自动弹出提示
3. 点安装即可

---

## 进阶：打包成真正的 .ipa / .apk（可选）

如果要上架 App Store / Google Play，需要用 Capacitor：

```bash
npm install -g @capacitor/cli
npm init @capacitor/app
npx cap add ios      # 需要 Mac + Xcode
npx cap add android  # 需要 Android Studio
```

把 `index.html` 放到 `www/` 文件夹，然后 `npx cap sync`。
上架需要苹果开发者账号（$99/年）或谷歌账号（$25 一次性）。

---

## 快捷演示（课堂 demo 用）

直接在手机浏览器打开下载的 `index.html` 文件也可以演示，
功能完全正常，只是 AI 生成需要 proxy。
