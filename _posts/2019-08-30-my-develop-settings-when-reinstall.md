---
layout: single
title: "[Notes] My develop settings when reinstall"
date: 2019-08-30
modified: 2020-01-01
description:
categories:
    - "Notes"
tags:
header:
---

Record my development environment

## Install Homebrew
```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Install develop tools >
```sh
brew cask install visual-studo-code
brew cask install iterm2 
brew cask install docker
brew tap caskroom/fonts
brew cask install font-sourcecodepro-nerd-font
brew cask install font-fira-code
brew install zsh
brew install python
brew install node
```

## Setting Zsh as default & install oh-my-zsh
```sh
sudo sh -c "echo $(which zsh) >> /etc/shells"
chsh -s $(which zsh)
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## iTerm settings
```sh
# Preferences > Profiles > Terminal > Report Terminal Type> xterm-256color
git clone https://github.com/mbadolato/iTerm2-Color-Schemes.git
# iTerm import Tomorrow Night Eighties & select
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k

vi ~/.zshrc
ZSH_THEME="powerlevel9k/powerlevel9k"
# Left
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(context dir dir_writable vcs vi_mode)
# Right
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status background_jobs time os_icon)
# Hide user name
DEFAULT_USER="your_username"
# nerd font
POWERLEVEL9K_MODE='nerdfont-complete'
```

## iTerm User Profile Json
```json
{
  "Ansi 4 Color" : {
    "Green Component" : 0.59999999999999998,
    "Red Component" : 0.40000000000000002,
    "Blue Component" : 0.80000000000000004
  },
  "Tags" : [

  ],
  "Ansi 12 Color" : {
    "Green Component" : 0.59999999999999998,
    "Red Component" : 0.40000000000000002,
    "Blue Component" : 0.80000000000000004
  },
  "Ansi 5 Color" : {
    "Green Component" : 0.59999999999999998,
    "Red Component" : 0.80000000000000004,
    "Blue Component" : 0.80000000000000004
  },
  "Use Non-ASCII Font" : false,
  "Bold Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 0.80000000000000004,
    "Blue Component" : 0.80000000000000004
  },
  "Ansi 6 Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 0.40000000000000002,
    "Blue Component" : 0.80000000000000004
  },
  "AWDS Tab Directory" : "",
  "Normal Font" : "SauceCodeProNerdFontComplete-Regular 16",
  "Rows" : 25,
  "Default Bookmark" : "No",
  "Ansi 2 Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 0.59999999999999998,
    "Blue Component" : 0.59999999999999998
  },
  "Ansi 3 Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 1,
    "Blue Component" : 0.40000000000000002
  },
  "AWDS Tab Option" : "No",
  "Cursor Guide Color" : {
    "Red Component" : 0.70213186740875244,
    "Color Space" : "sRGB",
    "Blue Component" : 1,
    "Alpha Component" : 0.25,
    "Green Component" : 0.9268307089805603
  },
  "Non-ASCII Anti Aliased" : true,
  "Use Bright Bold" : true,
  "Ansi 10 Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 0.59999999999999998,
    "Blue Component" : 0.59999999999999998
  },
  "Ambiguous Double Width" : false,
  "AWDS Pane Option" : "No",
  "Jobs to Ignore" : [
    "rlogin",
    "ssh",
    "slogin",
    "telnet"
  ],
  "Show Status Bar" : true,
  "Ansi 15 Color" : {
    "Green Component" : 0.99997437000274658,
    "Red Component" : 1,
    "Blue Component" : 0.99999129772186279
  },
  "Foreground Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 0.80000000000000004,
    "Blue Component" : 0.80000000000000004
  },
  "Working Directory" : "\/Users\/rickjiang",
  "Blinking Cursor" : false,
  "AWDS Window Option" : "No",
  "Sync Title" : false,
  "Prompt Before Closing 2" : false,
  "BM Growl" : true,
  "Command" : "",
  "Description" : "Default",
  "AWDS Pane Directory" : "",
  "Disable Window Resizing" : true,
  "Screen" : -1,
  "Selection Color" : {
    "Green Component" : 0.31764705882352939,
    "Red Component" : 0.31764705882352939,
    "Blue Component" : 0.31764705882352939
  },
  "Mouse Reporting" : true,
  "AWDS Window Directory" : "",
  "Columns" : 80,
  "Idle Code" : 0,
  "Ansi 13 Color" : {
    "Green Component" : 0.59999999999999998,
    "Red Component" : 0.80000000000000004,
    "Blue Component" : 0.80000000000000004
  },
  "Custom Command" : "No",
  "ASCII Anti Aliased" : true,
  "Non Ascii Font" : "Monaco 12",
  "Vertical Spacing" : 1,
  "Use Bold Font" : true,
  "Option Key Sends" : 0,
  "Selected Text Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 0.80000000000000004,
    "Blue Component" : 0.80000000000000004
  },
  "Background Color" : {
    "Green Component" : 0.1764705882352941,
    "Red Component" : 0.1764705882352941,
    "Blue Component" : 0.1764705882352941
  },
  "Character Encoding" : 4,
  "Ansi 11 Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 1,
    "Blue Component" : 0.40000000000000002
  },
  "Use Italic Font" : true,
  "Unlimited Scrollback" : false,
  "Keyboard Map" : {
    "0xf700-0x260000" : {
      "Action" : 10,
      "Text" : "[1;6A"
    },
    "0x37-0x40000" : {
      "Action" : 11,
      "Text" : "0x1f"
    },
    "0x32-0x40000" : {
      "Action" : 11,
      "Text" : "0x00"
    },
    "0xf709-0x20000" : {
      "Action" : 10,
      "Text" : "[17;2~"
    },
    "0xf70c-0x20000" : {
      "Action" : 10,
      "Text" : "[20;2~"
    },
    "0xf729-0x20000" : {
      "Action" : 10,
      "Text" : "[1;2H"
    },
    "0xf72b-0x40000" : {
      "Action" : 10,
      "Text" : "[1;5F"
    },
    "0xf705-0x20000" : {
      "Action" : 10,
      "Text" : "[1;2Q"
    },
    "0xf703-0x260000" : {
      "Action" : 10,
      "Text" : "[1;6C"
    },
    "0xf700-0x220000" : {
      "Action" : 10,
      "Text" : "[1;2A"
    },
    "0xf701-0x280000" : {
      "Action" : 11,
      "Text" : "0x1b 0x1b 0x5b 0x42"
    },
    "0x38-0x40000" : {
      "Action" : 11,
      "Text" : "0x7f"
    },
    "0x33-0x40000" : {
      "Action" : 11,
      "Text" : "0x1b"
    },
    "0xf703-0x220000" : {
      "Action" : 10,
      "Text" : "[1;2C"
    },
    "0xf701-0x240000" : {
      "Action" : 10,
      "Text" : "[1;5B"
    },
    "0xf70d-0x20000" : {
      "Action" : 10,
      "Text" : "[21;2~"
    },
    "0xf702-0x260000" : {
      "Action" : 10,
      "Text" : "[1;6D"
    },
    "0xf729-0x40000" : {
      "Action" : 10,
      "Text" : "[1;5H"
    },
    "0xf706-0x20000" : {
      "Action" : 10,
      "Text" : "[1;2R"
    },
    "0x34-0x40000" : {
      "Action" : 11,
      "Text" : "0x1c"
    },
    "0xf700-0x280000" : {
      "Action" : 11,
      "Text" : "0x1b 0x1b 0x5b 0x41"
    },
    "0x2d-0x40000" : {
      "Action" : 11,
      "Text" : "0x1f"
    },
    "0xf70e-0x20000" : {
      "Action" : 10,
      "Text" : "[23;2~"
    },
    "0xf702-0x220000" : {
      "Action" : 10,
      "Text" : "[1;2D"
    },
    "0xf703-0x280000" : {
      "Action" : 11,
      "Text" : "0x1b 0x1b 0x5b 0x43"
    },
    "0xf700-0x240000" : {
      "Action" : 10,
      "Text" : "[1;5A"
    },
    "0xf707-0x20000" : {
      "Action" : 10,
      "Text" : "[1;2S"
    },
    "0xf70a-0x20000" : {
      "Action" : 10,
      "Text" : "[18;2~"
    },
    "0x35-0x40000" : {
      "Action" : 11,
      "Text" : "0x1d"
    },
    "0xf70f-0x20000" : {
      "Action" : 10,
      "Text" : "[24;2~"
    },
    "0xf703-0x240000" : {
      "Action" : 10,
      "Text" : "[1;5C"
    },
    "0xf701-0x260000" : {
      "Action" : 10,
      "Text" : "[1;6B"
    },
    "0xf702-0x280000" : {
      "Action" : 11,
      "Text" : "0x1b 0x1b 0x5b 0x44"
    },
    "0xf72b-0x20000" : {
      "Action" : 10,
      "Text" : "[1;2F"
    },
    "0x36-0x40000" : {
      "Action" : 11,
      "Text" : "0x1e"
    },
    "0xf708-0x20000" : {
      "Action" : 10,
      "Text" : "[15;2~"
    },
    "0xf701-0x220000" : {
      "Action" : 10,
      "Text" : "[1;2B"
    },
    "0xf70b-0x20000" : {
      "Action" : 10,
      "Text" : "[19;2~"
    },
    "0xf702-0x240000" : {
      "Action" : 10,
      "Text" : "[1;5D"
    },
    "0xf704-0x20000" : {
      "Action" : 10,
      "Text" : "[1;2P"
    }
  },
  "Window Type" : 0,
  "Background Image Location" : "",
  "Blur" : false,
  "Badge Color" : {
    "Red Component" : 1,
    "Color Space" : "sRGB",
    "Blue Component" : 0,
    "Alpha Component" : 0.5,
    "Green Component" : 0.1491314172744751
  },
  "Scrollback Lines" : 1000,
  "Send Code When Idle" : false,
  "Close Sessions On End" : true,
  "Terminal Type" : "xterm-256color",
  "Visual Bell" : true,
  "Flashing Bell" : false,
  "Status Bar Layout" : {
    "components" : [
      {
        "class" : "iTermStatusBarCPUUtilizationComponent",
        "configuration" : {
          "knobs" : {
            "base: priority" : 5,
            "shared text color" : {
              "Red Component" : 0.90000000000000002,
              "Color Space" : "sRGB",
              "Blue Component" : 0.63,
              "Alpha Component" : 1,
              "Green Component" : 0.63
            },
            "base: compression resistance" : 1
          },
          "layout advanced configuration dictionary value" : {
            "font" : ".AppleSystemUIFont 12",
            "algorithm" : 0
          }
        }
      },
      {
        "class" : "iTermStatusBarMemoryUtilizationComponent",
        "configuration" : {
          "knobs" : {
            "base: priority" : 5,
            "shared text color" : {
              "Red Component" : 0.76500000000000001,
              "Color Space" : "sRGB",
              "Blue Component" : 0.63,
              "Alpha Component" : 1,
              "Green Component" : 0.90000000000000002
            }
          },
          "layout advanced configuration dictionary value" : {
            "font" : ".AppleSystemUIFont 12",
            "algorithm" : 0
          }
        }
      },
      {
        "class" : "iTermStatusBarBatteryComponent",
        "configuration" : {
          "knobs" : {
            "base: priority" : 5,
            "base: compression resistance" : 1,
            "shared text color" : {
              "Red Component" : 0.63,
              "Color Space" : "sRGB",
              "Blue Component" : 0.90000000000000002,
              "Alpha Component" : 1,
              "Green Component" : 0.90000000000000002
            }
          },
          "layout advanced configuration dictionary value" : {
            "font" : ".AppleSystemUIFont 12",
            "algorithm" : 0
          }
        }
      },
      {
        "class" : "iTermStatusBarSearchFieldComponent",
        "configuration" : {
          "knobs" : {
            "base: priority" : 5,
            "shared text color" : {
              "Red Component" : 0.76500000000000001,
              "Color Space" : "sRGB",
              "Blue Component" : 0.90000000000000002,
              "Alpha Component" : 1,
              "Green Component" : 0.63
            },
            "base: compression resistance" : 1
          },
          "layout advanced configuration dictionary value" : {
            "font" : ".AppleSystemUIFont 12",
            "algorithm" : 0
          }
        }
      }
    ],
    "advanced configuration" : {
      "font" : ".AppleSystemUIFont 12",
      "algorithm" : 0
    }
  },
  "Silence Bell" : false,
  "Ansi 14 Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 0.40000000000000002,
    "Blue Component" : 0.80000000000000004
  },
  "Name" : "Default",
  "Cursor Text Color" : {
    "Green Component" : 0.1764705882,
    "Red Component" : 0.1764705882,
    "Blue Component" : 0.1764705882
  },
  "Shortcut" : "",
  "Triggers" : [

  ],
  "Transparency" : 0,
  "Guid" : "3A8F2341-04C2-4566-A77F-1B9F8B1E3028",
  "Horizontal Spacing" : 1,
  "Custom Directory" : "Yes",
  "Cursor Color" : {
    "Green Component" : 0.80000000000000004,
    "Red Component" : 0.80000000000000004,
    "Blue Component" : 0.80000000000000004
  },
  "Right Option Key Sends" : 0,
  "Ansi 0 Color" : {
    "Green Component" : 0,
    "Red Component" : 0,
    "Blue Component" : 0
  },
  "Ansi 7 Color" : {
    "Green Component" : 0.99997437000274658,
    "Red Component" : 1,
    "Blue Component" : 0.99999129772186279
  },
  "Ansi 8 Color" : {
    "Green Component" : 0,
    "Red Component" : 0,
    "Blue Component" : 0
  },
  "Ansi 9 Color" : {
    "Green Component" : 0.46666666670000001,
    "Red Component" : 0.94901960780000005,
    "Blue Component" : 0.47843137250000001
  },
  "Ansi 1 Color" : {
    "Green Component" : 0.46666666666666667,
    "Red Component" : 0.94901960784313721,
    "Blue Component" : 0.47843137254901957
  },
  "Link Color" : {
    "Red Component" : 0,
    "Color Space" : "sRGB",
    "Blue Component" : 0.73423302173614502,
    "Alpha Component" : 1,
    "Green Component" : 0.35916060209274292
  }
}
```

## Setting Git & GitHub with SSH
```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
cd ~/.ssh
cat id_rsa.pub
ssh git@github.com
git config --global user.name "your_username"
git config --global user.email "your_email@example.com"
```