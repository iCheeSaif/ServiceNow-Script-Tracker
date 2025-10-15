# 🧭 NowTrack — ServiceNow Script Progress Tracker

Stop digging through logs or refreshing endlessly just to check your script’s status.  
**NowTrack** is a Chrome extension that connects to your ServiceNow instance and shows live progress for any background script. ⚡  

---

## 🔍 Get Started

1. Install **NowTrack** from the Chrome Web Store.  
2. Commit **one update set** to your instance — and you’re ready to go.  

---

## 🧠 Key Features

- 🟢 **Real-time tracking** — see processed vs total records instantly.  
- ⚙️ **Works with any** `*.service-now.com` instance.  
- ⚡ **Auto-refreshes** every 5 seconds — no need to reload.  
- 🧩 **Lightweight & setup-free** — plug and play.  
- 🎨 **Clean dark UI** with color-coded statuses *(Running / Done / Error)*.  

---

## 🪄 How to Use

1️⃣ Commit the provided update set to your instance.  
2️⃣ In your **server script** (Business Rule, Background Script, Fix Script, or any server-side code), call the `ScriptProgressTracker` Script Include and pass a custom prefix.  

💡 **Pro Tip:** Use a unique prefix for each script to keep trackers separate.  
Then open your Chrome extension and watch your script’s progress update in real-time. ⚙️✨  

---
<h3>## 💻 Example 1 — Simple GlideRecord</h3>


```javascript
(function() {
    var prefix = 'Incident Mass Update';
    var tracker = new ScriptProgressTracker(prefix);
    var gr = new GlideRecord('incident');
    gr.query();
    var total = gr.getRowCount();
    tracker.start(total);
    var processed = 0;
    while (gr.next()) {
        try {
            gr.active = false;
            gr.update();
            processed++;
            if (processed % 50 == 0) {
                tracker.step(50);
            }
        } catch (e) {
            tracker.fail(e.message);
            return;
        }
    }
    tracker.step(processed % 50);
    tracker.finish();
})();

 
 


 
