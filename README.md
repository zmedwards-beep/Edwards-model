<!DOCTYPE html>
<html lang="en-AU">
<head>
    <meta charset="UTF-8">
    <title>The Edwards Model Visualiser v2.1</title>
    <script src="https://cdn.jsdelivr.net"></script>
    <style>
        :root { --accent: #4ecca3; --bg: #1a1a2e; --panel: #16213e; }
        body { font-family: sans-serif; background: var(--bg); color: white; display: flex; flex-direction: column; align-items: center; padding: 20px; }
        .grid { display: grid; grid-template-columns: 1fr 1.5fr; gap: 20px; max-width: 1000px; width: 100%; }
        .panel { background: var(--panel); padding: 20px; border-radius: 12px; border: 1px solid #0f3460; }
        input[type=range] { width: 100%; accent-color: var(--accent); }
        .btn { background: var(--accent); color: #1a1a2e; border: none; padding: 10px; border-radius: 5px; cursor: pointer; width: 100%; font-weight: bold; margin-top: 10px; }
        #node-display { font-size: 2rem; color: var(--accent); margin: 10px 0; }
    </style>
</head>
<body>
    <h1>THE EDWARDS MODEL</h1>
    <div class="grid">
        <div class="panel">
            <h3>Matrix Controls</h3>
            <label>Serotonin (S)</label><input type="range" id="S" min="0" max="1" step="1" value="0" oninput="update()">
            <label>Dopamine (D)</label><input type="range" id="D" min="0" max="1" step="1" value="0" oninput="update()">
            <label>Noradrenaline (N)</label><input type="range" id="N" min="0" max="1" step="1" value="0" oninput="update()">
            <label>Oxytocin (O)</label><input type="range" id="O" min="0" max="1" step="1" value="0" oninput="update()">
            <label>Endorphins (E)</label><input type="range" id="E" min="0" max="1" step="1" value="0" oninput="update()">
            <button class="btn" onclick="saveGhost()">PIN COMPARISON</button>
            <button class="btn" style="background:#2d4263; color:white;" onclick="exportImg()">EXPORT IMAGE</button>
        </div>
        <div class="panel">
            <canvas id="radarChart"></canvas>
            <div style="text-align: center;">
                <div id="node-display">EMPTY VOID</div>
                <p id="delta-report" style="color: #a2a8d3;"></p>
            </div>
        </div>
    </div>
    <p>Authors: Z.M. Edwards, S.M. Edwards, O.S.R. Edwards | Woolgoolga, Australia</p>

<script>
const dict = {
    "0,0,0,0,0": "Empty Void", "1,1,1,1,1": "The Edwards Peak",
    "1,0,1,1,1": "Universal Sublime", "0,0,1,1,0": "Grief / Anguish",
    "1,1,1,0,1": "Flow State", "1,0,1,0,1": "Indomitable Resilience"
};
let ghost = null;
let ctx = document.getElementById('radarChart').getContext('2d');
let radar = new Chart(ctx, {
    type: 'radar',
    data: {
        labels: ['S', 'D', 'N', 'O', 'E'],
        datasets: [{ label: 'Current', data: [0,0,0,0,0], borderColor: '#4ecca3', backgroundColor: 'rgba(78,204,163,0.2)' },
                   { label: 'Target', data: null, borderColor: 'rgba(255,255,255,0.3)', hidden: true }]
    },
    options: { scales: { r: { min: 0, max: 1 } } }
});
function update() {
    const vals = [document.getElementById('S').value, document.getElementById('D').value, document.getElementById('N').value, document.getElementById('O').value, document.getElementById('E').value];
    radar.data.datasets[0].data = vals; radar.update();
    const node = dict[vals.join(',')] || "Hybrid State";
    document.getElementById('node-display').innerText = node;
}
function saveGhost() {
    ghost = [...radar.data.datasets[0].data];
    radar.data.datasets[1].data = ghost; radar.data.datasets[1].hidden = false;
    radar.update();
}
function exportImg() {
    const link = document.createElement('a');
    link.download = 'EdwardsModel_State.png';
    link.href = document.getElementById('radarChart').toDataURL();
    link.click();
}
</script>
</body>
</html>
