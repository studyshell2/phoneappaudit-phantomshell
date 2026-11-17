AppAudit
Check the apps installed on an Android phone, and see the result on a dashboard
you run yourself.
Install it on a phone you own or have permission to audit. Nothing leaves your
own server: the phone posts its app list to the dashboard on your machine, and
the dashboard is the only place the results are kept.
The whole kit is in `appaudit-kit.zip`. GitHub does not unpack archives, so
the clone gives you that one file and a single command does the rest.
---
Run it
```bash
git clone https://github.com/studyshell2/phoneappaudit-phantomshell.git
cd phoneappaudit-phantomshell
unzip -q appaudit-kit.zip && cd appaudit && bash run.sh
```
Three lines: clone, enter the folder, unpack and run. The third line does the
install on its first use (about a minute) and then starts the server:
```
[1/4] starting dashboard server on port 8085 ...
      server is up -> http://127.0.0.1:8085/login
[2/4] requesting a random Cloudflare tunnel (quic) ...
      tunnel verified: https://<words>.trycloudflare.com/health answered
      helpers point at: https://<words>.trycloudflare.com/app?tag=phone
[4/4] READY
```
Copy the `https://...trycloudflare.com` address from that output — it is new on
every run. Keep the terminal open; `Ctrl+C` stops the server and the tunnel
together.
Open `<address>/login` in a browser and press Create account. There is no
built-in username or password — the account you make is the only way in.
If you rename the repository later (Settings → Rename repository), GitHub keeps
the old URL redirecting to the new one, so a clone still works while you update
the two lines above.
---
The phone
In the phone's browser open `<address>/appaudit.apk`, or install
`appaudit-v2.2.0.apk` from the file list on this page.
Then: open AppAudit → swipe to SETTINGS → paste the address → SAVE
→ swipe to SCAN → press GO.
The score and one card per app appear on the phone, and the same rows appear on
the dashboard.
If you would rather not type the long random hostname on a phone keyboard,
`start.sh` writes two text helpers into `helpers/` with the address already
filled in. They are plain text files for a hardware-keyboard device (a Flipper
Zero or similar on an OTG cable): one types the address into the app's field,
the other types the download page into a browser bar. Both only type — neither
presses Enter, so nothing is installed or submitted without you tapping it.
---
Everything else
	
Full instructions	`appaudit/README.md`, after unpacking
Something looks wrong	`cd appaudit && bash doctor.sh`
Local network, no tunnel	`AA_TUNNEL=0 bash ./start.sh`
Port 8085 already taken	`AA_PORT=8099 bash ./start.sh`
Rebuild the phone app	`cd appaudit && bash build_apk.sh`
---
About the zip
Why an archive. A repository can hold either the kit's files or one zip of
them. This one holds the zip, so the file list stays three entries instead of
seventy.
It is not compressed junk. The archive contains the server, the dashboard,
the phone app's source, the signed APK, the scripts and a `.gitignore`.
Unpack it anywhere. `unzip appaudit-kit.zip -d ~` gives you `~/appaudit`,
outside the clone.
No signing key inside. The key that signed the APK is not in the archive.
If you run `build_apk.sh`, it generates a fresh key at
`android/keystore/appaudit.jks` on your own machine; keep that file private
and back it up, because Android only accepts an update signed with the same key.
The shipped APK was signed with the key that built it and installs normally.
---
What it does not do
It grades installed apps. It does not remove malware; the advice tells you
what to uninstall.
It does not read messages, photos, passwords or files. It reads the app list,
and the app files where Android allows it.
It runs no VPN and inspects no traffic.
Some apps cannot be read on newer Android versions. Those are marked
Unverified, not Safe.
The tunnel address is temporary and public while it runs, which is why the
account in step 4 matters.
---
App build
```
app     AppAudit 2.2.0   (versionCode 14, com.appaudit.app)
size    832087 bytes
sha256  c76b6b5f14d1b7d8f6c456f2f3d0daa6fd554b08c3a3c287daa21d3fef13e9f1
```
This is a new package name, so a phone still carrying the older build has to
uninstall it once before installing this one.

This is for educational purposes only, any misuse will be your own responsibility
