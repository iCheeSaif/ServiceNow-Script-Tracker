 ServiceNow Script Progress Tracker


 
 <img width="351" height="409" alt="image" src="https://github.com/user-attachments/assets/db1eb6db-ea24-4749-9355-d858e5b79b3b" />


A Chrome extension that connects directly to your ServiceNow instance and shows live progress for any running background script.
No more refreshing logs or digging through tables — just open the tracker, select your script, and watch the progress bar fill up in real time 🚀

🧠 Features

🟢 Real-time script progress (records processed / total)
⚙️ Works automatically with any *.service-now.com instance
🧩 Lightweight & setup-free — plug and play
🎨 Clean dark UI with color-coded statuses (running / done / error)

🪄 How to Use

1️⃣ Commit the update set included with the extension to your instance.
2️⃣ In your server-side script, call the Script Include named ScriptProgressTracker and pass your script prefix — for example:
"Incident Mass Update".

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

            // Update every 50 records
            if (processed % 50 == 0) {
                tracker.step(50);
            }
        } catch (e) {
            tracker.fail(e.message);
            return;
        }
    }

    // Final update for remaining records
    tracker.step(processed % 50);
    tracker.finish();
})();

💡 Tip

Keep the prefix unique per script to avoid mixing results in the tracker.
Then just open your Chrome extension popup — and enjoy watching real-time updates
