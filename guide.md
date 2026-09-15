# How to Install Custom JWPUB Files on an iPhone or iPad Using Your Mac (without Jailbreak)

**Difficulty:** Intermediate (6.5/10) — some experience with using Terminal and command-line commands is helpful.

**Goal:** To download and install an older version of JW Library (version 15.6), which can be used to import custom JWPUB files. Once the custom JWPUB files are installed, JW Library can be updated to the latest version, while the custom files remain in the app.

## What you need

- A Mac
- An iPhone or iPad
- A USB cable
- An Apple Account
- An internet connection

> ⚠️ **IMPORTANT:** Before proceeding, make sure to make a backup of your JW Library, which includes your notes, highlights and playlists. Even if the method described in this guide does not work for you, your backup will keep your data safe.

> 📌 **PLEASE NOTE:** The older JW Library file obtained through this process is downloaded from the official App Store and is tied to your personal Apple Account. Therefore, it cannot be shared with other people. Each person needs to obtain their own copy.

## 1. Install necessary tools

Search for and open Terminal on your Mac.

First, install Homebrew, which will allow us to install the other tools needed for this process.

Inside Terminal, run the following command:

```bash
if command -v brew >/dev/null 2>&1; then
  echo "Homebrew is already installed — skipping Homebrew installation."
else
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
fi
```

> 📌 **PLEASE NOTE:** If Homebrew is already installed, you'll see a message saying so and can continue. Otherwise, Homebrew will be installed on your Mac — this may take a while, and the installation may ask for your password. Enter your Mac password when prompted.

After Homebrew has been successfully installed, copy and paste both commands at once:

```bash
brew install ipatool
brew install -y ideviceinstaller
```

## 2. Log in to ipatool

In order to download the older version of the JW Library we need the ipatool. First, connect your device to your Mac using a USB cable. Unlock your device and, if prompted, choose Trust and follow the instructions on the screen.

Once connected, back inside Terminal, run:

```bash
ipatool auth login
```

Follow the instructions to sign in with your Apple Account.

> 📌 **PLEASE NOTE:** In this step ipatool communicates with Apple's servers to authenticate your account, and Apple might require two-factor authentication as part of the sign-in process.

Once you've successfully logged in, you can download the older version of JW Library directly from the App Store.

**For an iPhone** inside Terminal, run:

```bash
ipatool download --app-id 672417831 --external-version-id 878901217 --platform iphone --output ~/Downloads/JWLibrary-15.6.ipa --purchase
```

**For an iPad** inside Terminal, run:

```bash
ipatool download --app-id 672417831 --external-version-id 878901217 --platform ipad --output ~/Downloads/JWLibrary-15.6.ipa --purchase
```

The downloaded file will be saved in your Mac's Downloads folder. It should be named JWLibrary-15.6.ipa. Check to see if it's there.

## 3. Import JW Library 15.6 to your device

To install the older version of the JW Library we just downloaded, delete the currently installed JW Library app from your iPhone or iPad, if you have one installed. 

> ⚠️ **IMPORTANT:** Make sure you have successfully completed the backup described at the beginning of this guide.

After deleting the JW Library from your device, you are ready to install the older version.

Make sure your device is connected to your Mac. Inside Terminal, run the following command:

```bash
ideviceinstaller install ~/Downloads/JWLibrary-15.6.ipa
```

Check that JW Library installed properly on your iPhone or iPad — you should now be running version 15.6.

## 4. Install custom JWPUB files

You are now ready to install your custom JWPUB files to your device. If the files are already on your iPhone or iPad, you can import them manually, one by one, using the normal file-sharing/open-in-JW-Library process. However, to save time, you can also download these files to your computer and put them inside a folder of your choice. Then, inside Terminal, run the following command, which will allow you to transfer multiple JWPUB files at the same time. Your device should still be connected to your Mac.

```bash
folder=$(osascript -e 'POSIX path of (choose folder with prompt "Select folder containing JWPUB files:")')

for f in "$folder"*.jwpub; do
    [ -e "$f" ] || continue
    afcclient --documents org.jw.jwlibrary put "$f" "/Documents/$(basename "$f")"
done
```

> 📌 **PLEASE NOTE:** When prompted, select the folder where you have saved your custom JWPUB files. You may need to restart JW Library for the imported files to appear.

> 💡 **Tip:** This method can also be used to install larger JWPUB files, such as the Bible or the Insight book. First, download the files to your computer from the JW.org website, then use the above command line to import all of them to your device in one go. 

## ✅ Done

Once you have installed everything you need, you can safely update JW Library to the latest version — your custom JWPUB files will remain in place after the update. Also, don't forget to restore all your notes from your backup file.
