# ServiceNow-Script-Tracker
This Chrome extension connects directly to your ServiceNow instance and shows live progress for any running script. No need to refresh logs or dig through tables — just open the tracker, select your script, and watch the bar fill up in real time
<br>
<h1>How to use?</h1>
1- Commit the related update set to your instance.
2- at any server script, you need to call the script include named ScriptProgressTracker and pass your script prefix! for eample: Incident Mass Update

Background Example : 
<script>
  (function() {
    var prefix = 'incident Mass Update';
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

</script>

