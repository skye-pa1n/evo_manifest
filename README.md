# Vanilla Ice Cream Based (Android 15)
![Evolution X](https://github.com/Evolution-X/manifest/raw/udc/Banner.png)
# Manifest for building Evolution X for S20FE 

`r8q` is the codename for the Snapdragon variant of the Samsung S20FE, with global models being SM-G780G (4G) & SM-G781B (5G).

Some extremely basic instructions:

- Make a new directory for Evolution sources and enter it:
```
mkdir evo
cd evo
```

- Install Building Dependencies
```
sudo apt install bc bison build-essential curl flex g++-multilib gcc-multilib git gnupg gperf libxml2 lib32z1-dev liblz4-tool libncurses5-dev libsdl1.2-dev libwxgtk3.0-gtk3-dev imagemagick git lunzip lzop schedtool squashfs-tools xsltproc zip zlib1g-dev openjdk-8-jdk python3 perl xmlstarlet virtualenv xz-utils rr jq libncurses5 pngcrush lib32ncurses5-dev git-lfs libxml2 openjdk-11-jdk-headless rsync
```

- Install repo bin to system:
```
curl https://storage.googleapis.com/git-repo-downloads/repo > repo
chmod a+x repo
mv ./repo /usr/bin 
```
- Install ccache
```
apt install ccache
```
- Turn ccache on and set its size to 40Gb
```
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
ccache -M 40G
```

- Initialize repo in this directory with the Evolution X Android15 repository:
```
repo init --depth=1 -u https://github.com/Evolution-X/manifest -b vic --git-lfs
```

- Clone this repo to .repo/local_manifests for roomservice.xml containing the repositories with the device/vendor/hw trees needed to build for the S20 FE:
```
git clone https://github.com/skye-pa1n/evo_manifest.git -b r8q-vic .repo/local_manifests
```

- Sync all of the repositories in manifests (including EvolutionX manifests):
  
- Tip: Go get some sleep, or sit back and relax, because this will take a while depending on your internet connection.

- Also repo sync is run the second time just in case anything got borked and it didnt manage to fix in the first run, dont worry it wont take long the second time. 
```
repo sync -c -j8 --force-sync --no-clone-bundle --no-tags && repo sync -c -j8 --force-sync --no-clone-bundle --no-tags 
```

- These keys are VERY important, they are used to sign your build, without these you wont be able to pass BASIC & DEVICE integrity.
```
croot && git clone https://github.com/Evolution-X/vendor_evolution-priv_keys-template vendor/evolution-priv/keys
```

- Generate the keys.
```
cd vendor/evolution-priv/keys/
./keys.sh
```

- Finally, build as you like.
```
. build/envsetup.sh
lunch lineage_r8q-ap4a-userdebug
m evolution
```
Or
```
. build/envsetup.sh
lunch lineage_r8q-ap4a-user
m evolution
```
- Notes to self:
PLS dont be stoobid and do clean builds
```
. build/envsetup.sh
lunch lineage_r8q-ap4a-user
m clean
m evolution
```
Remove the previous manifest if needed
```
rm -rf .repo/local_manifests
```
- Notes:
  
Use lunch lineage_r8q-ap3a-userdebug if you want a userdebug build. (PLAY INTEGRITY WONT PASS)

Use lunch lineage_r8q-ap3a-user if you want a normal build. (Google Play Integrity will pass by default.)

Be patient, everything here does take a while.
