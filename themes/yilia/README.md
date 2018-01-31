# hexo-theme-yilia
原项目见[yilia](https://github.com/litten/hexo-theme-yilia)

## b-minihippo分支说明
#### markdown解析样式
由于强迫症，将原markdown显示的css样式改为了github favored css样式（不完全）。具体改变如下：
1. Headers
  - h1-h6都添加了下划线
  - h4 取消大写英文字符
  - 修改h1-h6的字体大小及上下间距
2. List
  - 修改list计数的表示方式（css样式）
  - 修改list字体大小
  - 修复嵌套list计数错误问题
  - 添加list嵌套内容的缩进
3. Task List
  - 修改task list的显示问题
  - 添加checkbox垂直对齐

#### bug修复
  - 修复当文章开始内容格式是h1时，header标题高度丢失问题

## yilia源码结构
hexo-theme-yilia
```shell
.
├── README.md
├── _config.yml
├── languages
├── layout
├── node_modules
├── package.json
├── source               # 编译结果
├── source-src           # 源码
└── webpack.config.js
```

source-src
```shell
source-src/
├── css
│   ├── core
│   ├── fonts
│   ├── img
│   ├── _core.scss
│   ├── _function.scss
│   ├── archive.scss        # 文章归类显示样式
│   ├── article-inner.scss  # 文章样式
│   ├── article-main.scss   # 文章除内容样式
│   ├── article-nav.scss    # 文章底部链接样式
│   ├── article.scss        # 文章样式
│   ├── aside.scss          # 返回顶部及toc
│   ├── comment.scss
│   ├── fonts.scss
│   ├── footer.scss         # 网页脚注样式
│   ├── global.scss
│   ├── grid.scss
│   ├── highlight.scss
│   ├── left.scss         # 左侧栏样式
│   ├── main.scss
│   ├── mobile-slider.scss
│   ├── mobile.scss
│   ├── page.scss         # 首页底部翻页栏
│   ├── reward.scss       # 赏金样式
│   ├── scroll.scss
│   ├── share.scss        # 文章底部右侧分享
│   ├── social.scss       # 左侧分享
│   ├── tags-cloud.scss
│   ├── tags.scss
│   ├── tools.scss        # 左边关于我等打开窗口样式
│   └── tooltip.scss      # 文章悬浮toc样式
├── css.ejs
├── js
│   ├── Q.js              # 快速url定向
│   ├── anm.js            # 初始化
│   ├── aside.js          # toc与返回顶部
│   ├── browser.js        # 判断浏览器类型
│   ├── fix.js            # 初始化分页等内容
│   ├── main.js
│   ├── mobile.js
│   ├── report.js
│   ├── share.js          # 文章分享
│   ├── slider.js         # 左侧滑动窗口
│   ├── util.js
│   └── viewer.js         # 图片显示
└── script.ejs
```

layout
```shell
layout
├── _partial
│   ├── after-footer.ejs
│   ├── archive-post.ejs
│   ├── archive.ejs
│   ├── article.ejs
│   ├── aside.ejs
│   ├── baidu-analytics.ejs
│   ├── css.ejs
│   ├── footer.ejs
│   ├── google-analytics.ejs
│   ├── head.ejs
│   ├── header.ejs
│   ├── left-col.ejs
│   ├── mathjax.ejs        # 公式显示
│   ├── mobile-nav.ejs
│   ├── post               # 评论
│   │   ├── category.ejs
│   │   ├── changyan.ejs
│   │   ├── date.ejs
│   │   ├── duoshuo.ejs
│   │   ├── gitment.ejs
│   │   ├── nav.ejs
│   │   ├── share.ejs
│   │   ├── tag.ejs
│   │   ├── title.ejs
│   │   └── wangyiyun.ejs
│   ├── script.ejs
│   ├── toc.ejs           # toc
│   ├── tools.ejs         # 返回顶部
│   └── viewer.ejs        # 显示图片
├── archive.ejs
├── category.ejs
├── index.ejs
├── layout.ejs
├── page.ejs
├── post.ejs
└── tag.ejs
```

