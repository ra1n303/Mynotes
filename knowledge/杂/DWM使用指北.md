

# 基础配置信息

```

/* 外观 */

static const unsigned int borderpx  = 1;        /* 设置边框 */
static const unsigned int snap      = 32;       /* 窗口边缘靠近屏幕边缘或是另一个窗口时，会自动对齐 */
static const int showbar            = 1;        /* 是否显示状态栏 0 表示不显示 */
static const int topbar             = 1;        /* 状态栏位置，1表示顶部，0表示底部 */
static const char *fonts[]          = { "Hack:size=10" }; /* 定义状态栏和窗口标题使用的字体 */
static const char dmenufont[]       = "Hack:size=10"; /* 定义dmenu起动器字体 */
static const char col_gray1[]       = "#222222"; /* 背景色 */
static const char col_gray2[]       = "#444444"; /* 边框色 */
static const char col_gray3[]       = "#bbbbbb"; /* 前景色，文字 */
static const char col_gray4[]       = "#eeeeee";  /* 选中文字色 */
static const char col_cyan[]        = "#005577";  /* 选中背景色 */
static const char *colors[][3]      = {
	            /*  	  前景色     背景色      边框色   */
/* 普通状态 */	[SchemeNorm] = { col_gray3, col_gray1, col_gray2 },
/* 选中状态 */	[SchemeSel]  = { col_gray4, col_cyan,  col_cyan  },
};
/* 定义颜色方案，应用于状态栏，窗口标题和边框 */

/* 标签设置 */
/* 定义DWM的工作区（标签）用于组织窗口 */
static const char *tags[] = { "1", "2", "3", "4", "5", "6", "7", "8", "9" };

/* 定义窗口规则，根据窗口的类，实例或标题自动分配标签或行为 */
static const Rule rules[] = {
	/* xprop(1):
	 *	WM_CLASS(STRING) = instance, class
	 *	WM_NAME(STRING) = title
	 */
	/* class      instance    title       tags mask     isfloating   monitor */
	{ "Gimp",     NULL,       NULL,       0,            1,           -1 },
	{ "Firefox",  NULL,       NULL,       1 << 8,       0,           -1 },
};


/* 布局设置  */
static const float mfact     = 0.55; /* 主窗口区域的宽度比例 [0.05..0.95] */
static const int nmaster     = 1;    /* 主窗口区域的窗口数量，如果需要堆叠多个窗口，可改为2 */
static const int resizehints = 1;    /* 是否尊重窗口的尺寸提示（1表示开启） */
static const int lockfullscreen = 1; /* 全屏窗口是否聚焦锁定焦点 */

/* 定义可用布局模式 */
static const Layout layouts[] = {
	{ "[]=",      tile },    /*  平铺布局，默认，主窗口在左侧，其他欻功能口平铺在右侧*/
	{ "><>",      NULL },    /* 浮动窗口，窗口可自由移动 */
	{ "[M]",      monocle }, /* 单窗口布局，所有窗口全屏显示 */
};

/* 快捷键定义 */
#define MODKEY Mod1Mask  /* 设置主修饰键，当前值为Mod1Mask,即Alt */ /*Mod4Mask表示win*/
#define TAGKEYS(KEY,TAG) \
	{ MODKEY,                       KEY,      view,           {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask,           KEY,      toggleview,     {.ui = 1 << TAG} }, \
	{ MODKEY|ShiftMask,             KEY,      tag,            {.ui = 1 << TAG} }, \
	{ MODKEY|ControlMask|ShiftMask, KEY,      toggletag,      {.ui = 1 << TAG} },
/* 宏定义，用于快速设置标签相关的快捷键 */
/* MODKEY + key切换到指定标签 */
/* MODKEY + Control + key 切换标签的可见性 */
/* MONKEY + Shift + key 将丹铅窗口移动到指定标签 */
/* MODKEY + Control + Shirf + KEY  切换窗口的标签标记*/

/* 宏定义，用于简化shell命令的执行 */
/* 将命令cmd封装为/bin/sh -c cmd */
#define SHCMD(cmd) { .v = (const char*[]){ "/bin/sh", "-c", cmd, NULL } }

/* 记录dmenu的显示器编号，当前值为0,默认在主显示器上显示dmemu */
static char dmenumon[2] = "0";

/* dmenu_run:运行dmenu */
/* -m 0 ： 运行在住显示器*/
/* -fn 字体 */
/* -nb 背景色 */
/* -nf 前景色 */
/* -sb 选中背景色 */
/* -sf 选中前景色 */
static const char *dmenucmd[] = { "dmenu_run", "-m", dmenumon, "-fn", dmenufont, "-nb", col_gray1, "-nf", col_gray3, "-sb", col_cyan, "-sf", col_gray4, NULL };

/* 定义终端启动命令 */
/* 当前为Alacritty */
static const char *termcmd[]  = { "alacritty", NULL };

/* 定义快捷键绑定 */
static const Key keys[] = {
	/* modifier                     key        function        argument */
	{ MODKEY,                       XK_p,      spawn,          {.v = dmenucmd } }, /* 启动dmenu */
	{ MODKEY|ShiftMask,             XK_Return, spawn,          {.v = termcmd } }, /* 启动终端 */
	{ MODKEY,                       XK_b,      togglebar,      {0} }, /* 切换状态栏显示/隐藏 */
	{ MODKEY,                       XK_j,      focusstack,     {.i = +1 } }, /* 焦点切换到下一个窗口 */
	{ MODKEY,                       XK_k,      focusstack,     {.i = -1 } }, /* 焦点切换到上一个窗口 */
	{ MODKEY,                       XK_i,      incnmaster,     {.i = +1 } }, /* 增加主窗口数量 */
	{ MODKEY,                       XK_d,      incnmaster,     {.i = -1 } }, /* 减少主窗口数量 */
	{ MODKEY,                       XK_h,      setmfact,       {.f = -0.05} }, /* 缩小主窗口区域 */
	{ MODKEY,                       XK_l,      setmfact,       {.f = +0.05} }, /* 扩大主窗口区域 */
	{ MODKEY,                       XK_Return, zoom,           {0} }, /* 将当前窗口放大为主窗口 */
	{ MODKEY,                       XK_Tab,    view,           {0} },  /* 切换到上一个标签 */
	{ MODKEY|ShiftMask,             XK_c,      killclient,     {0} },  /* 关闭当前窗口 */
	{ MODKEY,                       XK_t,      setlayout,      {.v = &layouts[0]} }, /* 切换到平铺布局 */
	{ MODKEY,                       XK_f,      setlayout,      {.v = &layouts[1]} }, /* 切换到浮动布局 */
	{ MODKEY,                       XK_m,      setlayout,      {.v = &layouts[2]} }, /* 切换到单窗口布局 */
	{ MODKEY,                       XK_space,  setlayout,      {0} },  /* 切换布局（循环） */
	{ MODKEY|ShiftMask,             XK_space,  togglefloating, {0} },  /* 切换当前窗口浮动状态 */
	{ MODKEY,                       XK_0,      view,           {.ui = ~0 } }, /* 查看所有标签 */
	{ MODKEY|ShiftMask,             XK_0,      tag,            {.ui = ~0 } }, /* 将当前窗口标记到所有标签 */
	{ MODKEY,                       XK_comma,  focusmon,       {.i = -1 } }, /* 焦点切换到上一个显示器 */
	{ MODKEY,                       XK_period, focusmon,       {.i = +1 } }, /* 焦点切换到下一个显示器 */
	{ MODKEY|ShiftMask,             XK_comma,  tagmon,         {.i = -1 } }, /* 将当前窗口移动到上一个显示器 */
	{ MODKEY|ShiftMask,             XK_period, tagmon,         {.i = +1 } }, /* 将当前窗口移动到下一个显示器 */
	TAGKEYS(                        XK_1,                      0)
	TAGKEYS(                        XK_2,                      1)
	TAGKEYS(                        XK_3,                      2)
	TAGKEYS(                        XK_4,                      3)
	TAGKEYS(                        XK_5,                      4)
	TAGKEYS(                        XK_6,                      5)
	TAGKEYS(                        XK_7,                      6)
	TAGKEYS(                        XK_8,                      7)
	TAGKEYS(                        XK_9,                      8)
	/* 切换到对应标签 */
	{ MODKEY|ShiftMask,             XK_q,      quit,           {0} },
	/* 推出DWM */
};

/* 鼠标点击行为定义 */
/* 定义鼠标点击在不同区域（如状态栏，窗口标题的行为） */
static const Button buttons[] = {
	/* click                event mask      button          function        argument */
	{ ClkLtSymbol,          0,              Button1,        setlayout,      {0} },
	{ ClkLtSymbol,          0,              Button3,        setlayout,      {.v = &layouts[2]} },
	{ ClkWinTitle,          0,              Button2,        zoom,           {0} },
	{ ClkStatusText,        0,              Button2,        spawn,          {.v = termcmd } },
	{ ClkClientWin,         MODKEY,         Button1,        movemouse,      {0} },
	{ ClkClientWin,         MODKEY,         Button2,        togglefloating, {0} },
	{ ClkClientWin,         MODKEY,         Button3,        resizemouse,    {0} },
	{ ClkTagBar,            0,              Button1,        view,           {0} },
	{ ClkTagBar,            0,              Button3,        toggleview,     {0} },
	{ ClkTagBar,            MODKEY,         Button1,        tag,            {0} },
	{ ClkTagBar,            MODKEY,         Button3,        toggletag,      {0} },
};

/* ClkLtSymbol 布局符号 */
/* ClkWinTitle 窗口标题 */
/* ClkStatusText 状态栏文本 */
/* ClkClientWin 客户端窗口 */
/* ClkTagBar 标签栏 */

/* Button1,左键 */
/* Button2,中键 */
/* Button3,右键 */
```





# 补丁

配置设置

```
git config user.email "2368383583@qq.com"
git config user.name "ra1n333"
git config --global merge.tool kdiff3
```



```
git branch
git branch xxx
git checkout xxx
patch -i
git add xxx xxx xxxfiele
git commit  -m xxx
git checkout master
git merge xxx -m xxx
cp config.def.h config.h
make install clean 
```

