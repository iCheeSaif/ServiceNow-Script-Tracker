<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/3df30a66-964e-4303-b196-43325053715b" />

NowTrack — ServiceNow Script Progress Tracker

Tired of refreshing logs or scrolling endlessly through tables just to see how your script is doing?
Meet NowTrack — a Chrome extension that connects directly to your ServiceNow instance and shows live progress for any running background script ⚡

🔍 Get Started

Search & install: NowTrack Chrome Extension
Commit one update set to your instance — and you’re good to go!

🧠 Key Features

🟢 Real-time progress tracking — see processed vs total records instantly
⚙️ Works out of the box with any *.service-now.com instance
⚡ Auto-refreshes every 5 seconds — no manual refresh needed
🧩 Lightweight & setup-free — plug and play simplicity
🎨 Dark, minimal UI with color-coded statuses (Running / Done / Error)

🪄 How to Use

1️⃣ Commit the provided update set to your instance
2️⃣ In your server-side script, call the ScriptProgressTracker Script Include and pass a custom prefix

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


💡 Pro Tip:
Use a unique prefix for each script to avoid overlap in your tracker.
Then open your Chrome extension — and watch your script progress update live in real time ⚙️✨
