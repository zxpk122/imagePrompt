# Vercel 部署指南

## 🚨 构建失败？环境变量错误？

如果您看到以下构建错误：
```
❌ Invalid environment variables: {
  NEXTAUTH_SECRET: [ 'Required' ],
  GITHUB_CLIENT_ID: [ 'Required' ],
  GITHUB_CLIENT_SECRET: [ 'Required' ],
  STRIPE_API_KEY: [ 'Required' ],
  STRIPE_WEBHOOK_SECRET: [ 'Required' ],
  NEXT_PUBLIC_APP_URL: [ 'Required' ]
}
```

**立即解决方案：** 请查看 [VERCEL_ENV_SETUP.md](./VERCEL_ENV_SETUP.md) 获取详细的环境变量配置指南。

## 构建错误解决方案

如果在Vercel部署时遇到"无效的环境变量"错误，这是因为缺少必需的环境变量。请按照以下步骤配置：

## 必需的环境变量

### 1. 基础配置
```bash
NEXT_PUBLIC_APP_URL=https://your-app-name.vercel.app
NEXTAUTH_URL=https://your-app-name.vercel.app
NEXTAUTH_SECRET=your-nextauth-secret-here
```

### 2. GitHub OAuth（必需）
```bash
GITHUB_CLIENT_ID=your-github-client-id
GITHUB_CLIENT_SECRET=your-github-client-secret
```

**获取方式：**
1. 访问 [GitHub Developer Settings](https://github.com/settings/developers)
2. 创建新的 OAuth App
3. 设置 Authorization callback URL 为：`https://your-app-name.vercel.app/api/auth/callback/github`

### 3. Stripe 配置（必需）
```bash
STRIPE_API_KEY=sk_live_your-stripe-secret-key
STRIPE_WEBHOOK_SECRET=whsec_your-stripe-webhook-secret
```

**获取方式：**
1. 访问 [Stripe Dashboard](https://dashboard.stripe.com/)
2. 获取 API 密钥
3. 创建 Webhook 端点：`https://your-app-name.vercel.app/api/webhooks/stripe`

### 4. 可选配置
```bash
# Stripe 产品 ID（可选但推荐）
NEXT_PUBLIC_STRIPE_PRO_PRODUCT_ID=prod_your-pro-product-id
NEXT_PUBLIC_STRIPE_PRO_MONTHLY_PRICE_ID=price_your-pro-monthly-price-id
NEXT_PUBLIC_STRIPE_PRO_YEARLY_PRICE_ID=price_your-pro-yearly-price-id
NEXT_PUBLIC_STRIPE_BUSINESS_PRODUCT_ID=prod_your-business-product-id
NEXT_PUBLIC_STRIPE_BUSINESS_MONTHLY_PRICE_ID=price_your-business-monthly-price-id
NEXT_PUBLIC_STRIPE_BUSINESS_YEARLY_PRICE_ID=price_your-business-yearly-price-id

# PostHog 分析（可选）
NEXT_PUBLIC_POSTHOG_KEY=your-posthog-key
NEXT_PUBLIC_POSTHOG_HOST=https://app.posthog.com
```

## 在 Vercel 中配置环境变量

1. 登录 [Vercel Dashboard](https://vercel.com/dashboard)
2. 选择你的项目
3. 进入 **Settings** → **Environment Variables**
4. 添加上述所有必需的环境变量
5. 确保为 **Production**、**Preview** 和 **Development** 环境都添加变量

## 快速部署步骤

1. **Fork 或克隆项目**
2. **配置环境变量**（按照上述指南）
3. **在 Vercel 中导入项目**
4. **等待自动部署完成**

## 故障排除

### 构建失败："无效的环境变量"
- 确保所有必需的环境变量都已在 Vercel 中配置
- 检查环境变量名称是否正确（区分大小写）
- 确保没有多余的空格或特殊字符

### GitHub OAuth 错误
- 确保 GitHub OAuth App 的回调 URL 正确
- 检查 `GITHUB_CLIENT_ID` 和 `GITHUB_CLIENT_SECRET` 是否正确

### Stripe 错误
- 确保使用的是正确的 Stripe API 密钥（生产环境使用 `sk_live_`）
- 检查 Webhook 端点是否正确配置

## 注意事项

- 🔒 **安全性**：永远不要在代码中硬编码敏感信息
- 🌍 **环境**：生产环境使用 `live` 密钥，测试环境使用 `test` 密钥
- 📝 **文档**：保持环境变量文档更新
- 🔄 **备份**：定期备份重要的配置信息

## 联系支持

如果遇到问题，请检查：
1. Vercel 部署日志
2. 环境变量配置
3. 第三方服务（GitHub、Stripe）的配置

部署成功后，你的 SaaS 应用将在 `https://your-app-name.vercel.app` 上运行！