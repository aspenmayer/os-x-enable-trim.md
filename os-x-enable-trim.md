# Enable TRIM on non-Apple SSDs in OS X

NOTE: This is tested on the versions mentioned in the title, and NOT earlier or later versions. YMMV.

Run the following commands in Terminal…

1. Backup the original driver

    sudo cp /System/Library/Extensions/IOAHCIFamily.kext/Contents/PlugIns/IOAHCIBlockStorage.kext/Contents/MacOS/IOAHCIBlockStorage /System/Library/Extensions/IOAHCIFamily.kext/Contents/PlugIns/IOAHCIBlockStorage.kext/Contents/MacOS/IOAHCIBlockStorage-backup

2. Modify the driver (choose only one of the following, based on the version)

    # 10.8.3 to 10.9.2
    sudo perl -pi -e 's|(\x52\x6F\x74\x61\x74\x69\x6F\x6E\x61\x6C\x00{1,20})[^\x00]{9}(\x00{1,20}\x54)|$1\x00\x00\x00\x00\x00\x00\x00\x00\x00$2|sg' /System/Library/Extensions/IOAHCIFamily.kext/Contents/PlugIns/IOAHCIBlockStorage.kext/Contents/MacOS/IOAHCIBlockStorage

    # 10.7.5, 10.8.1, 10.8.2
    sudo perl -pi -e 's|(\x52\x6F\x74\x61\x74\x69\x6F\x6E\x61\x6C\x00{1,20})[^\x00]{9}(\x00{1,20}\x4D)|$1\x00\x00\x00\x00\x00\x00\x00\x00\x00$2|sg' /System/Library/Extensions/IOAHCIFamily.kext/Contents/PlugIns/IOAHCIBlockStorage.kext/Contents/MacOS/IOAHCIBlockStorage

3. Run these commands in succession to clear the system caches to enable OS X to pick up the modified driver

    sudo kextcache -system-prelinked-kernel
    sudo kextcache -system-caches
    sudo touch /System/Library/Extensions/

4. Restart the Mac