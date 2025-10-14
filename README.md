<img width="351" height="409" alt="image" src="https://github.com/user-attachments/assets/9350edcf-658e-4959-b7e1-1fb5e668287e" />




ServiceNow Script Progress Tracker

NowTrack — a Chrome extension that hooks straight into your ServiceNow instance and shows live progress for any running background script.
No more refreshing logs or scrolling through tables — just open the tracker, pick your script, and watch the bar move in real time ⚡

🔍 Get Started

Search & install: NowTrack Chrome Extension

🧠 Features

🟢 Real-time script progress — see records processed vs total

⚙️ Works out of the box with any *.service-now.com instance

🧩 Lightweight & setup-free — pure plug and play

🎨 Dark, minimal UI with color-coded statuses (Running / Done / Error)

🪄 How to Use

1️⃣ Commit the provided update set to your instance.
2️⃣ In your server-side script, call the Script Include ScriptProgressTracker and pass a custom prefix — for example: "Incident Mass Update"

💻 Example Background Script
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

💡 Pro Tip

Keep each script’s prefix unique to avoid overlap in the tracker.
Then just pop open your Chrome extension — and enjoy real-time progress, the clean way. 🚀
