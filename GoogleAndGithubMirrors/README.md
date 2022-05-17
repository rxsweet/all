# GoogleAndGithubMirrors
使用 Cloudflare Workers 搭建 Google 和 GitHub 镜像网站

学习大佬的方法，不错

https://progcz.com/posts/use-cf-workers-build-google-and-github-mirrors/


=======================================================================================

对于 Google 来说，虽然大部分时间都能够在科学环境中网上冲浪，但是难免有需要在普通环境中使用 Google 的情况，而对于 GitHub 来说，虽然目前可以无障碍访问，但是在普通环境中下载某些 Release（比如科学工具的客户端）的速度实在太慢。

如果你也有和我一样的困扰，那么可以考虑使用 Cloudflare Workers 搭建属于自己的镜像网站，在普通环境中备用。

0 写在前面
如果你只是在寻找临时的解决方案，而又不想费劲的话，那么可以直接使用我已经搭建好的镜像网站。

但是，请你务必遵守以下约定：

*不滥用服务。因为每个 Cloudflare 账户每天只有 100,000 次请求的额度。

*不登录自己的任何账号。虽然我保证不拦截你的数据，但是防人之心不可无。

*不违反大陆的法律法规。虽然你需要科学，但是请保持理性。

Google 镜像网站：https://google.progcz.workers.dev/

GitHub 镜像网站：https://github.progcz.workers.dev/

1 注册并登录 Cloudflare 账号
这没啥好说的，前往 Cloudflare 官网自行注册并登录，然后点击「Workers」。

2 创建新的 Worker 应用
进入 Workers 页面之后，新用户需要设置用户名（比如 progcz），然后点击「创建 Worker」。

3 部署 Worker 应用
自行修改应用名（比如 test），将 index.js 中的代码拷贝至脚本中，点击「保存并部署」，然后就可以通过 https://test.progcz.workers.dev/（注意替换应用名 test 和用户名 progcz）访问 Google 的镜像网站了。

4 自定义 index.js 脚本
上文的 index.js 其实来自于 Berkeley-Reject/Workers-Proxy 仓库，但是代码中设置了对于国内访问的屏蔽，所以为了避免误用，我就在自行修改之后保存了一份。

可以通过修改以下部分来搭建不同的镜像网站：

// Website you intended to retrieve for users.
const upstream = 'www.google.com'

// Custom pathname for the upstream website.
const upstream_path = '/'

// Website you intended to retrieve for users using mobile devices.
const upstream_mobile = 'www.google.com'

// Countries and regions where you wish to suspend your service.
const blocked_region = ['KP', 'SY', 'PK', 'CU']

// IP addresses which you wish to block from using your service.
const blocked_ip_address = ['0.0.0.0', '127.0.0.1']

// Whether to use HTTPS protocol for upstream address.
const https = true

// Whether to disable cache.
const disable_cache = false

// Replace texts.
const replace_dict = {
    '$upstream': '$custom_domain',
    '//google.com': ''
}
需要注意的是，上述代码只是 index.js 的一小部分。

比如，对于 GitHub 来说，我们只需要将 upstream、upstream_mobile 和 replace_dict 中的 google.com 修改为 github.com 即可。
