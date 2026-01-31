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
-- config.color_scheme = 'Aurora'
config.color_scheme = 'Catppuccin Mocha'
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
config.default_prog = { 'C:\\Users\\97766\\AppData\\Local\\Programs\\nu\\bin\\nu.exe' }


-------------------- 键盘绑定 --------------------
local act = wezterm.action

-- 改用tmux
-- config.leader = { key = 'a', mods = 'CTRL', timeout_milliseconds = 1000 }
-- config.keys = {
--   { key = 'q',          mods = 'LEADER',         action = act.QuitApplication },
  
--   { key = 'h',          mods = 'LEADER',         action = act.SplitHorizontal { domain = 'CurrentPaneDomain' } },
--   { key = 'v',          mods = 'LEADER',         action = act.SplitVertical { domain = 'CurrentPaneDomain' } },
--   { key = 'q',          mods = 'CTRL',           action = act.CloseCurrentPane { confirm = false } },
  
--   { key = 'LeftArrow',  mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Left' },
--   { key = 'RightArrow', mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Right' },
--   { key = 'UpArrow',    mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Up' },
--   { key = 'DownArrow',  mods = 'SHIFT|CTRL',     action = act.ActivatePaneDirection 'Down' },

config.keys = {
  -- CTRL + T 创建默认的Tab 
  { key = 't', mods = 'CTRL', action = act.SpawnTab 'DefaultDomain' },
  -- ALT + W 关闭当前Tab
  { key = 'w', mods = 'ALT', action = act.CloseCurrentTab { confirm = false } },

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
      domain = 'DefaultDomain',
      args = {'wsl', '-d', 'kali-linux'},
    }
  },
  {
    key = '#',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      domain = 'DefaultDomain',
      args = {'wsl', '-d', 'Ubuntu'},
    }
  },
  {
    key = '$',
    mods = 'CTRL|SHIFT',
    action = act.SpawnCommandInNewTab {
      args = {'cmd'},
    }
  },
  {
    key = '%',
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
--       File = 'D:/壁纸/黑色2.png',
--     },
--   }
-- }

return config
```

## nushell

路径：`nushell` 中执行 `echo $nu.config-path`

```
# Nushell Config File
#
# version = "0.110.1"
$env.config.color_config = {
    separator: default
    leading_trailing_space_bg: { attr: n }
    header: green_bold
    empty: blue
    bool: light_cyan
    int: default
    filesize: cyan
    duration: default
    datetime: purple
    range: default
    float: default
    string: default
    nothing: default
    binary: default
    cell-path: default
    row_index: green_bold
    record: default
    list: default
    closure: green_bold
    glob:cyan_bold
    block: default
    # hints: dark_gray
    # 自动补全字符颜色
    hints: '#6c6c6c'
    search_result: { bg: red fg: default }
    shape_binary: purple_bold
    shape_block: blue_bold
    shape_bool: light_cyan
    shape_closure: green_bold
    shape_custom: green
    shape_datetime: cyan_bold
    shape_directory: cyan
    shape_external: cyan
    shape_externalarg: green_bold
    shape_external_resolved: light_yellow_bold
    shape_filepath: cyan
    shape_flag: blue_bold
    shape_float: purple_bold
    shape_glob_interpolation: cyan_bold
    shape_globpattern: cyan_bold
    shape_int: purple_bold
    shape_internalcall: cyan_bold
    shape_keyword: cyan_bold
    shape_list: cyan_bold
    shape_literal: blue
    shape_match_pattern: green
    shape_matching_brackets: { attr: u }
    shape_nothing: light_cyan
    shape_operator: yellow
    shape_pipe: purple_bold
    shape_range: yellow_bold
    shape_record: cyan_bold
    shape_redirection: purple_bold
    shape_signature: green_bold
    shape_string: green
    shape_string_interpolation: cyan_bold
    shape_table: blue_bold
    shape_variable: purple
    shape_vardecl: purple
    shape_raw_string: light_purple
    shape_garbage: {
        fg: default
        bg: red
        attr: b
    }
}

# ========================================= 自定义配置 =========================================
# 自定义 PROMPT
$env.PROMPT_COMMAND_RIGHT = ""
$env.PROMPT_COMMAND = {||
    let parts = (pwd | path split)

    let display = if ($parts | length) > 3 {
        "..\\" + ($parts | last 3 | path join)
    } else {
        ($parts | path join)
    }

    $"($display) "
}

# 不显示启动信息
$env.config.show_banner = false;
# 避免输入回车出现位移情况
$env.config.shell_integration.osc133 = false;

# 命令补全
use ~/AppData/Roaming/nushell/custom-completions/git/git-completions.nu *
use ~/AppData/Roaming/nushell/custom-completions/uv/uv-completions.nu *

# 定义别名和目录常量
alias vim = nvim
const ToolsDir = "F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\"
const ScriptDir = "F:\\CTF\\ProgramFiles\\CTF\\CTF_Script\\"
const WSLDir = "C:\\Users\\97766\\Downloads\\WSL\\"

# 设置代理
# $env.HTTP_PROXY = ""
def --env "proxy set" [] {
    load-env { "HTTP_PROXY": "socks5://127.0.0.1:10808", "HTTPS_PROXY": "socks5://127.0.0.1:10808" }
}

def --env "proxy set8083" [] {
    load-env { "HTTP_PROXY": "socks5://127.0.0.1:8083", "HTTPS_PROXY": "socks5://127.0.0.1:8083" }
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

# 1.工具路径别名
alias phpstan = D:\phpstudy_pro\Extensions\php\php7.4.3nts\php.exe "F:/CTF/ProgramFiles/CTF/CTF_APP/Web/静态分析/phpstan.phar"
alias MemProcFS = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\取证\\MemProcFS\\MemProcFS.exe
alias mimikatz = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\取证\\mimikatz_trunk\\x64\\mimikatz.exe
alias bruteHASH = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\爆破\\bruteHASH-master\\bruteHASH.exe
alias bkcrack = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\爆破\\bkcrack-1.7.1-win64\\bkcrack.exe
alias jsteg = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\图片\\jsteg\\jsteg.exe
alias bftools = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\图片\\braintools-v2.1\\bftools.exe
alias PNGDebugger = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\图片\\png-debugger-master\\Debug\\PNGDebugger.exe
alias pngcheck = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\图片\\pngcheck-3.0.3-win32\\pngcheck.win64.exe
alias stegdetect = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\图片\\stegdetect\\stegdetect.exe
alias snow = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\文本\\snwdos32\\SNOW.EXE
alias upx = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\逆向\\upx-4.1.0-win64\\upx.exe
alias ZXingReader = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\ZXing\\ZXingReader.exe
alias ZXingWriter = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\ZXing\\ZXingWriter.exe

# 3.音频工具
alias private_bit = D:\\Python\\python.exe $"($ScriptDir)3.Audio/private_bit/main.py"

# 4.其他脚本工具
alias VolatilityPro = D:\\Python\\python.exe $"($WSLDir)VolatilityPro/volpro.py"
alias jpgdetect = D:\\Python\\python.exe $"($WSLDir)JpgTools/detect.py"
alias crc32 = D:\\Python\\python.exe $"($ScriptDir)1.Compression/CRC-Tools/main.py"
alias VigenereTools = D:\\Python\\python.exe $"($ScriptDir)/../Vigenere-Tools/main.py"
alias pyinstxtractor-ng = F:\\CTF\\ProgramFiles\\CTF\\CTF_Tools\\逆向\\pyinstxtractor-ng\\pyinstxtractor-ng.exe

# 5.Traffic
alias BehinderBuster = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/TrafficDecrypter/BehinderBuster.py"
alias GodzillaBuster = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/TrafficDecrypter/GodzillaBuster.py"
alias AntSwordBuster = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/TrafficDecrypter/AntSwordBuster.py"
alias CobalStrikeBuster = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/TrafficDecrypter/CobalStrikeBuster.py"
alias KeyBuster = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/TrafficDecrypter/KeyBuster.py"

alias SQLBuster = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/SQLBuster/SQLBuster.py"
alias SQLogBuster = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/SQLBuster/SQLogBuster.py"
alias SQLParse = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/SQLBuster/SQLParse.py"

alias inject-tls-secrets = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/tls-decryption/inject-tls-secrets.py"

alias keyboard = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/USB-Tools/keyboard.py"
alias mouse = D:\\Python\\python.exe $"($ScriptDir)5.Traffic/USB-Tools/mouse.py"

alias BaseSeries = D:\\Python\\python.exe $"($ScriptDir)BaseSeries/main.py"

alias task = D:\task_windows_amd64\task.exe
```

