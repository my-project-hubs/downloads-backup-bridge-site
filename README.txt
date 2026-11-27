DOWNLOADS BACKUP BRIDGE - SITE REPO (PUBLIC) - YEH REPO KYON HAI
==================================================================

SABSE PEHLE: YEH APP KA REPO NAHI HAI
--------------------------------------
Is project ke 2 alag repo hain. Confuse mat hona.

1) APP REPO (PRIVATE)
   - Isme Android app ka pura code hai (downloads-backup-bridge.apk).
   - Java files: MainActivity, BridgeService, Drive, Store, BootReceiver.
   - GitHub Actions (.github/workflows/build.yml) se APK build hota hai.
   - Yeh repo PRIVATE rehna chahiye. Isse public mat karna.

2) SITE REPO (PUBLIC)  <-- YEH WALA (jisme yeh README hai)
   - Isme sirf 2 simple web page hain: index.html aur privacy.html.
   - Sirf Google Cloud ki zaroorat ke liye banaya gaya hai.
   - Isme koi app code, password, Client ID ya Client secret NAHI hai.
   - Yeh GitHub Pages se host hota hai (free account par Pages ke liye
     repo public hona zaroori hai, isliye alag repo banaya).

APP KYA KAAM KARTA HAI
----------------------
Mobile ke Downloads folder ki files kabhi kabhi delete ho jati hain.
App Downloads folder mein aane wali nayi file ko apne aap Google Drive ke
"Downloads-Bridge" folder mein upload kar deta hai. Server nahi hai, app
seedha phone se Google Drive se baat karta hai. Isliye website se nahi,
Android app se hi hoga.

Google se login "Device code" tareeke se hota hai:
  - App mein Client ID + Client secret daalte hain
  - "Google se jodo" dabate hain
  - Scope sirf: https://www.googleapis.com/auth/drive.file
    (app sirf apni banayi hui files/folder dekh sakta hai, poori Drive nahi)

PROBLEM KYA AAYI THI
--------------------
Google Cloud Console mein OAuth app abhi "Testing" status mein hai.
Testing mein refresh token 7 din mein expire ho jata hai, to backup
band ho jayega. Isliye app ko "In production" (Publish app) karna hai.

Par "Publish app" button grey tha. Wajah (Google ne khud bataya):
  "Valid app name, support email, homepage URL and privacy policy URL
   are required for switching the app to external production mode."
App name aur email bhare the, par Homepage URL aur Privacy Policy URL
khali the.

Hamare paas koi website nahi hai (Vercel account nahi hai). App repo
private hai, usse Pages nahi chalta. Isliye yeh alag public site repo
banaya, jisse Google ko 2 link mil jayen.

IS REPO KI FILES
----------------
index.html   - Homepage (app ka naam + chhota description + privacy link)
privacy.html - Privacy Policy (kya data lete hain, sharing nahi, kaise hatayein)
README.txt   - Yeh file (developer ke samajhne ke liye)
Dono HTML mein koi personal info (email, number) nahi rakhi hai.

PURA SETUP - STEP BY STEP
-------------------------
A) SITE REPO (yeh repo)
   1. GitHub par naya PUBLIC repo banao (jaise: bridge-site).
   2. Yeh teen files upload karo (zip nahi, extract karke), Commit karo.
   3. Settings -> Pages -> Deploy from a branch -> main -> /(root) -> Save.
   4. 1-2 minute baad link: https://USERNAME.github.io/bridge-site/
      Privacy link:        https://USERNAME.github.io/bridge-site/privacy.html

B) GOOGLE CLOUD CONSOLE (project: downloads-backup-bridge)
   1. Google Auth Platform -> Branding:
        App name, User support email, Developer contact email (bhare hain)
        Application home page        = A.4 wala link
        Application privacy policy   = A.4 wala privacy link
        Save
   2. Audience -> Publish app -> Confirm. Status "In production" hona chahiye.
   3. Data Access -> Add scope: .../auth/drive.file
   4. APIs & Services -> Library -> "Google Drive API" -> Enable.
   5. Clients -> Create client -> Application type:
        "TV and Limited Input devices"   (yahi type zaroori hai kyunki
        app Device code flow use karta hai)
      Isse Client ID aur Client secret milenge.

C) ANDROID APP
   1. APK install karo (GitHub Actions build se milta hai).
   2. App kholo -> Client ID aur Client secret daalo.
   3. "GOOGLE SE JODO" dabao -> screen par code/link aayega -> Google
      account se allow karo.
   4. Ab Downloads mein aane wali file apne aap Drive ke
      "Downloads-Bridge" folder mein jayegi.
      App ke do counter: "Mobile > App" aur "App > Google Drive".

SECRET KAHAN NA DAALEN
----------------------
Client secret kisi bhi GitHub repo (khaaskar public) mein mat daalna.
Woh sirf app ke andar phone par daalna hai.

KAUN SE REPO CHAHIYE (SAARANSH)
-------------------------------
1. App repo (private)  - code + APK build
2. Site repo (public)  - sirf yeh 3 files, Google Cloud ke Home page /
                          Privacy policy link ke liye
Aur koi repo, Vercel ya server ki zaroorat nahi.
