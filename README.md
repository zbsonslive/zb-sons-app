# Z.B & Sons — Paper Import & Godown Manager

App by: Raghib Mendha

Yeh app do cheezon per chalti hai:
1. **Firebase Firestore** — cloud database, jahan aap ka data save hota hai (shipments, inventory, parties, bank, sales sab).
2. **GitHub Pages** — free web hosting, jahan yeh app ek real URL per live ho jati hai jise aap kisi bhi device/browser se khol sakte hain.

Neeche step-by-step tareeqa hai.

---

## Step 1 — Firebase project banayein (5 minute)

1. https://console.firebase.google.com per jayein aur apne Google account se login karein.
2. **"Add project"** per click karein, naam dein (e.g. `zb-sons`), aur baaki defaults ke sath project bana lein.
3. Left menu mein **Build > Firestore Database** per jayein.
4. **"Create database"** per click karein.
5. Location select karein (jo aap ke qareeb ho), aur mode mein **"Start in test mode"** select karein (baad mein rules tabdeel karenge).
6. Ab project ke gear icon (⚙️) > **Project settings** per jayein.
7. Neeche "Your apps" section mein `</>` (web) icon per click karein, ek nickname dein (e.g. `zb-app`), aur **"Register app"** karein.
8. Aap ko ek code block milega jisme `firebaseConfig` object hoga — kuch aisa:
   ```js
   const firebaseConfig = {
     apiKey: "AIza...",
     authDomain: "zb-sons.firebaseapp.com",
     projectId: "zb-sons",
     storageBucket: "zb-sons.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
9. Is **poore object ko copy** kar lein.

## Step 2 — `index.html` mein config paste karein

1. `index.html` file kholein (kisi bhi text editor mein, ya GitHub per hi seedha edit karein — Step 3 dekhein).
2. File mein `YOUR_API_KEY`, `YOUR_PROJECT_ID`, `YOUR_SENDER_ID`, `YOUR_APP_ID` likhe hue mil jayenge — inhein apni asal Firebase config se replace kar dein (jo Step 1.8 mein copy ki thi).
3. Save kar dein.

## Step 3 — GitHub per upload karein

1. https://github.com per account banayein (agar pehle se nahi hai).
2. **"New repository"** per click karein, naam dein (e.g. `zb-sons-app`), **Public** rakhein, aur **"Create repository"** karein.
3. Us naye repo ke andar **"uploading an existing file"** link per click karein (ya "Add file > Upload files").
4. `index.html` file ko drag-and-drop karein aur **"Commit changes"** per click karein.

## Step 4 — GitHub Pages enable karein (yahan se aap ka live link banega)

1. Repo ke andar **Settings** tab per jayein.
2. Left menu mein **Pages** per click karein.
3. "Branch" ke neeche `main` select karein aur folder `/ (root)` rakhein, phir **Save** karein.
4. Kuch minute intezar karein — GitHub ek link degi jaisa: `https://yourusername.github.io/zb-sons-app/`
5. Yeh link ab aap ka **live app** hai — isay kisi bhi mobile, laptop, ya tablet ke browser mein khol sakte hain.

## Step 5 — Firestore security rules tight karein (zaroori)

Test mode sirf 30 din ke liye khula rehta hai aur is dauran koi bhi (jise database ka naam pata ho) data padh/likh sakta hai. Production ke liye:

1. Firebase Console > Firestore Database > **Rules** tab per jayein.
2. Yeh rules likhein (sirf `zbsons/main` document per read/write allow, baaki sab band):
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /zbsons/main {
         allow read, write: if true;
       }
     }
   }
   ```
3. **Publish** kar dein.

> Note: Yeh rules abhi bhi "koi bhi jise config pata ho woh data dekh/badal sakta hai" wali hain — asal mein app ka Admin/Viewer login sirf UI level per hai, Firestore level per nahi. Agar aap chahte hain ke database level per bhi mehfooz ho (sirf aap ke logged-in Admin hi likh sakein), tu Firebase Authentication add karni parhegi — yeh agla upgrade ho sakta hai, bata dein tu add kar deta hoon.

---

## Data kahan save hota hai?

Sara data ek hi Firestore document (`zbsons/main`) mein JSON ki soorat mein save hota hai — sab devices jo yeh link kholte hain unhein real-time mein wohi updated data dikhta hai. Login (Admin/Viewer) sirf yeh control karta hai ke edit/delete kaun kar sakta hai.
