---
title: "Setting up my macOs development environment for 2022"
datePublished: Mon Jan 03 2022 11:33:12 GMT+0000 (Coordinated Universal Time)
cuid: ckxylteew03h7bas17sa2gvfi
slug: setting-up-my-macos-development-environment-for-2022
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/LJ9KY8pIH3E/upload/v1641209453639/JC21HezFi.jpeg
tags: beginners, macos

---

By the third or fourth week of November was the only time I realized I was hacked. Tinkering on my mac’s logs and system files showed that the attackers started observing what I was doing between the last week of Febrary to the first week of March. Ill try to write all four exploits that the attackers did in a different blog post. In the meantime, this article details the custom “hack” I set on my mac for additional protection. This may or may not work for everyone. Duplicate at yer own risk. 

Things I did:

- Install XCode and CLI tools
- Deactivate remote management ``sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -deactivate -stop``
- Remove Desktop Sharing `sudo /System/Library/CoreServices/RemoteManagement/ARDAgent.app/Contents/Resources/kickstart -deactivate -configure -access -off`
- Remove Apple Remote Desktop Settings

```bash
sudo rm -rf /var/db/RemoteManagement ; \
sudo defaults delete /Library/Preferences/com.apple.RemoteDesktop.plist ; \
defaults delete ~/Library/Preferences/com.apple.RemoteDesktop.plist ; \
sudo rm -r /Library/Application\ Support/Apple/Remote\ Desktop/ ; \
rm -r ~/Library/Application\ Support/Remote\ Desktop/ ; \
rm -r ~/Library/Containers/com.apple.RemoteDesktop
```

- Uninstall Google Update `~/Library/Google/GoogleSoftwareUpdate/GoogleSoftwareUpdate.bundle/Contents/Resources/ksinstall --nuke`
- Show mail attachments as icons `defaults write com.apple.mail DisableInlineAttachmentViewing -bool yes`
- Enable developer menu and web inspector in safari

```bash
defaults write com.apple.Safari IncludeInternalDebugMenu -bool true && \
defaults write com.apple.Safari IncludeDevelopMenu -bool true && \
defaults write com.apple.Safari WebKitDeveloperExtrasEnabledPreferenceKey -bool true && \
defaults write com.apple.Safari com.apple.Safari.ContentPageGroupIdentifier.WebKit2DeveloperExtrasEnabled -bool true && \
defaults write -g WebKitDeveloperExtras -bool true
```

- Focus follows mouse on terminal `defaults write com.apple.Terminal FocusFollowsMouse -string YES`
- Use plain text in TextEdit as default `defaults write com.apple.TextEdit RichText -int 0`
- Disable local backups `sudo tmutil disable`

```bash
User@user ~ % sudo tmutil disable
tmutil: disable requires Full Disk Access privileges.
To allow this operation, select Full Disk Access in the Privacy
tab of the Security & Privacy preference pane, and add Terminal
to the list of applications which are allowed Full Disk Access.
```

- Install CLI Tools `xcode-select --install`
- Disable icon bounce on Dock

```bash
defaults write com.apple.dock no-bouncing -bool false && \
killall Dock
```

- Enable Scroll Gestures

```bash
defaults write com.apple.dock scroll-to-open -bool true && \
killall Dock
```

- Show Hidden Apps/Icons

```bash
defaults write com.apple.dock showhidden -bool true && \
killall Dock
```

- Disable Sudden Motion Sensor `sudo pmset -a sms 0`
- Show AFP, SMB, NFS, WebDAV etc

```bash
defaults write com.apple.finder ShowMountedServersOnDesktop -bool true && \
killall Finder
```

- Show All File Extensions `defaults write -g AppleShowAllExtensions -bool true`
- Show hidden files `defaults write com.apple.finder AppleShowAllFiles true`
- Show ~/Library folder `chflags nohidden ~/Library`
- Save to Disk by Default(not iCloud) `defaults write -g NSDocumentSaveNewDocumentsToCloud -bool false`
- Disable creation of .DS_Store and AppleDouble files `defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true`
- Recursively delete .DS_Store Files `find . -type f -name '.DS_Store' -ls -delete`
- Clear Font Cache for All users

```bash
sudo atsutil databases -removeUser && \
sudo atsutil server -shutdown && \
sudo atsutil server -ping
```

- Disable IR Receiver `sudo defaults write /Library/Preferences/com.apple.driver.AppleIRController DeviceEnabled -int 0`
- Disable sound effects on boot `sudo nvram SystemAudioVolume=" "`
- Disable autoplay in quicktime `defaults write com.apple.QuickTimePlayerX MGPlayMovieOnOpen`0
- Disable bonjour service `sudo defaults write /System/Library/LaunchDaemons/com.apple.mDNSResponder.plist ProgramArguments -array-add "-NoMulticastAdvertisements"`
- Enable screensaver password `defaults write com.apple.screensaver askForPassword -int 1`
- Install Homebrew `/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"`
- Install pyenv https://github.com/pyenv/pyenv
- Install nvm https://github.com/nvm-sh/nvm#installing-and-updating

No VSCode, right? I decided to use GitHub codespaces instead of coding in my Mac. I’ll most-likely write another article to talk about my GitHub codespaces set-up. Cheers!