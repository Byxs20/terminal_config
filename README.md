# terminal_config

## wezterm

路径：`C:\Users\用户名\.wezterm.lua`

```lua
-- 加载 wezterm API 和获取 config 对象
local wezterm = require 'wezterm'
local config = wezterm.config_builder()

config.launch_menu = {
  {
    label = 'PoserShell',
    args = { 'powershell.exe', '-NoLogo' },
  },
  {
    label = "Cmd",
    args = { 'cmd.exe'}
  }
}

-------------------- 颜色配置 --------------------
config.color_scheme = 'tokyonight_moon'
config.window_decorations = "TITLE | RESIZE"
config.use_fancy_tab_bar = false
config.enable_tab_bar = true
config.show_tab_index_in_tab_bar = true
config.hide_tab_bar_if_only_one_tab = false

config.inactive_pane_hsb = {
  saturation = 0.9,
  brightness = 0.8,
}

-- 设置字体和窗口大小
config.font = wezterm.font("CaskaydiaCove Nerd Font")
config.font_size = 12
config.initial_cols = 140
config.initial_rows = 30

-- 设置默认的启动shell

-- COMSPEC 是 Windows 系统的一个重要环境变量，通常默认指向 cmd.exe。覆盖它可能导致其他应用程序启动时依赖 COMSPEC 的功能出现异常。
-- config.set_environment_variables = {
--   COMSPEC = 'C:\\Users\\97766\\AppData\\Local\\Programs\\nu\\bin\\nu.exe',
-- }

config.default_prog = { 'C:\\Users\\97766\\AppData\\Local\\Programs\\nu\\bin\\nu.exe' }


-------------------- 键盘绑定 --------------------
local act = wezterm.action

config.leader = { key = 'a', mods = 'CTRL', timeout_milliseconds = 1000 }
config.keys = {
  { key = 'q',          mods = 'LEADER',         action = act.QuitApplication },

  { key = 'h',          mods = 'LEADER',         action = act.SplitHorizontal { domain = 'CurrentPaneDomain' } },
  { key = 'v',          mods = 'LEADER',         action = act.SplitVertical { domain = 'CurrentPaneDomain' } },
  { key = 'q',          mods = 'CTRL',           action = act.CloseCurrentPane { confirm = false } },

  { key = 'LeftArrow',  mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Left' },
  { key = 'RightArrow', mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Right' },
  { key = 'UpArrow',    mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Up' },
  { key = 'DownArrow',  mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Down' },

  -- CTRL + T 创建默认的Tab
  { key = 't', mods = 'CTRL', action = act.SpawnTab 'DefaultDomain' },
  -- CTRL + W 关闭当前Tab
  { key = 'w', mods = 'CTRL', action = act.CloseCurrentTab { confirm = false } },

  -- CTRL + SHIFT + 1 创建新Tab - WSL
  {
    key = '!',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      domain = 'DefaultDomain',
      args = {'wsl', '-d', 'Ubuntu-20.04'},
    }
  },
  {
    key = '@',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      args = {'cmd'},
    }
  },
  {
    key = '#',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      domain = 'DefaultDomain',
      args = {'pwsh'},
    }
  }
}

for i = 1, 8 do
  -- CTRL + number to activate that tab
  table.insert(config.keys, {
    key = tostring(i),
    mods = 'CTRL',
    action = act.ActivateTab(i - 1),
  })
end

-------------------- 鼠标绑定 --------------------
config.mouse_bindings = {
  -- copy the selection
  {
    event = { Up = { streak = 1, button = 'Left' } },
    mods = 'NONE',
    action = act.CompleteSelection 'ClipboardAndPrimarySelection',
  },

  -- Open HyperLink
  {
    event = { Up = { streak = 1, button = 'Left' } },
    mods = 'CTRL',
    action = act.OpenLinkAtMouseCursor,
  },
}

-------------------- 窗口居中 --------------------
wezterm.on("gui-startup", function(cmd)
        local screen = wezterm.gui.screens().active
        local width, height = screen.width * 0.5, screen.height * 0.5
        local tab, pane, window = wezterm.mux.spawn_window(cmd or {
                position = {
            x = (screen.width - width) / 2,
            y = (screen.height - height) / 2,
            origin = {Named=screen.name}
        }
        })
        window:gui_window():set_inner_size(width, height)
end)

-- 设置窗口透明度
-- config.window_background_opacity = 0.9
-- config.macos_window_background_blur = 10
-- config.background = {
--   {
--     source = {
--       File = 'D:/壁纸/wallhaven-858lz1_2560x1600.png',
--     },
--   }
-- }

return config
```

## nushell

路径：`nushell` 中执行 `echo $nu.config-path`

```
# 启动starship
use ~/.cache/starship/init.nu

# 定义别名和目录常量
alias vim = nvim

# 设置代理
# $env.HTTP_PROXY = ""
def --env "proxy set" [] {
    load-env { "HTTP_PROXY": "socks5://127.0.0.1:10808", "HTTPS_PROXY": "socks5://127.0.0.1:10808" }
}

proxy set

def --env "proxy unset" [] {
    load-env { "HTTP_PROXY": "", "HTTPS_PROXY": "" }
}

def "proxy check" [] {
    print "Try to connect to Google..."
    let resp = (curl -I -s --connect-timeout 2 -m 2 -w "%{http_code}" -o /dev/null www.google.com)
    
    if $resp == "200" {
        print "Proxy setup succeeded!"
    } else {
        print "Proxy setup failed!"
    }
}
```

## starship

路径：`~/.config/starship.toml`

打开 https://starship.rs/zh-CN/presets/ 配置一个主题即可，其他的设置自己查看文档
