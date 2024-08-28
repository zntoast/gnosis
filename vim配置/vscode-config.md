## setting.json 文件配置
```
{
    "editor.fontLigatures": false,
    "vim.easymotion": true,
    "vim.incsearch": true,
    "vim.useSystemClipboard": true,
    "vim.useCtrlKeys": true,
    "vim.hlsearch": true,
    "vim.scroll": 5,
    "vim.insertModeKeyBindings": [
        {
            "before": [],
            "after": []
        }
    ],
    "vim.normalModeKeyBindingsNonRecursive": [
        {
            "before": ["L"],
            "after": ["g","_"]
        },
        {
            "before": ["H"],
            "after": ["g","^"]
        },
        {
            "before": ["C-j"],
            "after": ["C-d"]
        },
        {
            "before": ["C-k"],
            "after": ["C-u"]
        },
        {
            "before": ["Q"],
            "commands": ["workbench.action.closeActiveEditor"],
            "silent": true
        },
        {
            "before": ["K"],
            "commands": ["lineBreakInsert"],
            "silent": true
        },
        {
            "before": ["S"],
            "commands": ["workbench.action.files.save"],
            "silent": true
        },
        {
            "before": ["<leader>","f"],
            "commands": [
                "workbench.action.quickOpen",
            ],
            "silent": true
        },
        {
            "before": ["<leader>","e"],
            "commands": [
                "workbench.action.toggleSidebarVisibility",
            ],
            "silent": true
        },
        {
            "before": ["<leader>","t"],
            "commands": [
                "workbench.action.togglePanel",
            ],
            "silent": true
        },
        {
            "before": ["<leader>","1"],
            "commands": ["workbench.action.openEditorAtIndex1"],
            "silent": true
        },
        {
            "before": ["<leader>","2"],
            "commands": ["workbench.action.openEditorAtIndex2"],
            "silent": true
        },
        {
            "before": ["<leader>","3"],
            "commands": ["workbench.action.openEditorAtIndex3"],
            "silent": true
        },
        {
            "before": ["<leader>","4"],
            "commands": ["workbench.action.openEditorAtIndex4"],
            "silent": true
        },
        {
            "before": ["<leader>","5"],
            "commands": ["workbench.action.openEditorAtIndex5"],
            "silent": true
        },
        {
            "before": ["<leader>","6"],
            "commands": ["workbench.action.openEditorAtIndex6"],
            "silent": true
        },
        {
            "before": ["<leader>","7"],
            "commands": ["workbench.action.openEditorAtIndex7"],
            "silent": true
        },
    ],
    "vim.visualModeKeyBindingsNonRecursive": [
        {
            "before": ["L"],
            "after": ["g","_"]
        },
        {
            "before": ["H"],
            "after": ["g","^"]
        },
        {
            "before": ["p"],
            "after": ["p","g","v","y"]
        },
        {
            "before": [
                ">"
            ],
            "commands": [
                "editor.action.indentLines"
            ]
        },
        {
            "before": [
                "<"
            ],
            "commands": [
                "editor.action.outdentLines"
            ]
        },
    ],
    "vim.leader": ",",
    "vim.handleKeys": {
        "<C-a>": false,
        "<C-f>": false
    },
    
    "vim.statusBarColorControl": true,
    "vim.statusBarColors.normal": ["#8FBCBB", "#434C5E"],
    "vim.statusBarColors.insert": "#BF616A",
    "vim.statusBarColors.visual": "#B48EAD",
    "vim.statusBarColors.visualline": "#B48EAD",
    "vim.statusBarColors.visualblock": "#A3BE8C",
    "vim.statusBarColors.replace": "#D08770",
    "vim.statusBarColors.commandlineinprogress": "#007ACC",
    "vim.statusBarColors.searchinprogressmode": "#007ACC",
    "vim.statusBarColors.easymotionmode": "#007ACC",
    "vim.statusBarColors.easymotioninputmode": "#007ACC",
    "vim.statusBarColors.surroundinputmode": "#007ACC",

    // 高亮选中色
    "vim.searchHighlightTextColor": "#F8F8FF",
    // easymotionPlug color
    "vim.easymotionMarkerBackgroundColor": "#00868B",
    "workbench.colorCustomizations": {
        "statusBar.background": "#8FBCBB",
        "statusBar.noFolderBackground": "#8FBCBB",
        "statusBar.debuggingBackground": "#8FBCBB",
        "statusBar.foreground": "#434C5E",
        "statusBar.debuggingForeground": "#434C5E"
    },
    "editor.semanticTokenColorCustomizations": {
    
    
    },
    "editor.tokenColorCustomizations": {
        "textMateRules": [
            {
                "scope": [
                    "annotation.xidl",
                ],
                "settings": {
                    "foreground": "#777777"
                }
            }
        ]
    },
    "explorer.confirmDelete": false,
    "git.confirmSync": false,
    "editor.fontVariations": false,
    "settingsSync.ignoredExtensions": [
    
    ],
    "go.toolsManagement.autoUpdate": true,
    "php.validate.executablePath": "",
    "docker.commands.build": "${containerCommand} build --pull --rm -f \"${dockerfile}\" -t ${tag} \"${context}\"",
    "workbench.colorTheme": "Default Dark+",
    "editor.suggest.showStatusBar": true,
    "extensions.experimental.affinity": {
        "asvetliakov.vscode-neovim": 1
    },
    "editor.codeActionsOnSave": {
        
    },
    "workbench.settings.applyToAllProfiles": [
    
    ],
    "codeium.enableConfig": {
        "*": false
    },
    "git.ignoreRebaseWarning": true

}
```
