
<!DOCTYPE html>
<html lang="hi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Meditation Studio | Hindi Subtitle Editor</title>

  <style>
    :root {
      --bg: #101510;
      --panel: #1d251e;
      --gold: #e9c77b;
      --border: #465342;
      --text: #f4f0df;
      --muted: #b8c1b3;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      background: var(--bg);
      color: var(--text);
      font-family: Arial, sans-serif;
      line-height: 1.6;
    }

    header {
      padding: 35px 18px;
      text-align: center;
      background: linear-gradient(135deg, #263b2c, #76552c);
    }

    header h1 { margin: 0 0 8px; }

    header p { margin: 0; }

    main {
      width: min(100% - 28px, 950px);
      margin: 25px auto;
    }

    .panel {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 13px;
      padding: 22px;
      margin-bottom: 20px;
    }

    h2, h3 { color: var(--gold); }

    h2 { margin-top: 0; }

    label {
      display: block;
      margin: 12px 0 5px;
      font-weight: bold;
    }

    input, textarea, select {
      width: 100%;
      padding: 12px;
      border: 1px solid #64705e;
      border-radius: 7px;
      background: #111711;
      color: white;
      font: inherit;
    }

    input[type="file"] { padding: 10px; }

    input[type="range"] { padding: 0; }

    audio { width: 100%; margin-top: 15px; }

    button {
      padding: 11px 15px;
      margin: 7px 5px 0 0;
      border: 0;
      border-radius: 7px;
      background: #c79b4b;
      color: #15150f;
      font-weight: bold;
      cursor: pointer;
    }

    button:hover { background: #e4bd71; }

    button.secondary {
      background: #344435;
      color: white;
      border: 1px solid #61705d;
    }

    button.danger {
      background: #642f2b;
      color: white;
    }

    .hint, .muted { color: var(--muted); font-size: 14px; }

    .grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 12px;
    }

    .subtitle-row {
      border: 1px solid var(--border);
      border-radius: 9px;
      padding: 14px;
      margin: 14px 0;
      background: #151c16;
    }

    .subtitle-row.active {
      border-color: var(--gold);
      box-shadow: 0 0 0 1px var(--gold);
    }

    .row-heading {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
    }

    .preview {
      background: #080a08;
      min-height: 125px;
      display: flex;
      justify-content: center;
      align-items: center;
      text-align: center;
      border: 1px solid #3d493b;
      border-radius: 10px;
      padding: 20px;
    }

    #previewText {
      font-size: clamp(20px, 4vw, 32px);
      font-weight: bold;
      color: white;
      text-shadow: 2px 2px 4px black;
      overflow-wrap: anywhere;
    }

    .counter {
      display: flex;
      flex-wrap: wrap;
      gap: 14px;
      color: var(--muted);
      margin: 12px 0;
    }

    footer {
      padding: 24px;
      text-align: center;
      color: var(--muted);
    }

    @media (max-width: 600px) {
      .grid { grid-template-columns: 1fr; }
      .panel { padding: 15px; }
      .row-heading { align-items: flex-start; }
    }
  </style>
</head>

<body>
  <header>
    <h1>🪷 Meditation Studio</h1>
    <p>Hindi Transcript & Subtitle Editor</p>
    <p class="muted">Apne audio ko subtitles ke saath taiyar karein</p>
  </header>

  <main>
    <section class="panel">
      <h2>1. Audio Upload</h2>

      <label for="audioFile">Apni audio recording chunein</label>
      <input type="file" id="audioFile" accept="audio/*">

      <audio id="audioPlayer" controls preload="metadata"></audio>

      <div class="counter">
        <span>Duration: <strong id="duration">00:00</strong></span>
        <span>Current: <strong id="currentTime">00:00</strong></span>
      </div>

      <p class="hint">
        Audio aapke browser mein hi play hota hai.
        Badi files load hone mein samay le sakti hain.
      </p>
    </section>

    <section class="panel">
      <h2>2. Hindi Transcript</h2>

      <label for="transcript">Apne pravachan ka transcript likhein</label>
      <textarea id="transcript" rows="7"
        placeholder="Yahan apna Hindi transcript paste karein...&#10;&#10;Jaise: Shanti bahar nahi, aapke bheetar hai."></textarea>

      <p class="hint">
        Har line se ek subtitle banega. Pehle audio ka transcript
        khud paste karein. Automatic speech recognition is version mein
        shamil nahi hai.
      </p>

      <button id="generateBtn">Lines se subtitles banayein</button>
      <button class="secondary" id="appendBtn">Maujooda subtitles ke aage jodein</button>
    </section>

    <section class="panel">
      <h2>3. Subtitle Timing</h2>

      <div class="grid">
        <div>
          <label for="defaultDuration">Har subtitle ka samay (seconds)</label>
          <input id="defaultDuration" type="number" min="1" max="30" value="4">
        </div>
        <div>
          <label for="gapDuration">Subtitles ke beech gap (seconds)</label>
          <input id="gapDuration" type="number" min="0" max="10" value="0">
        </div>
      </div>

      <button id="retimeBtn">Sabki timing dobara set karein</button>
      <button class="secondary" id="addBtn">+ Subtitle jodein</button>
      <button class="danger" id="clearBtn">Sab subtitles mitayein</button>

      <div class="counter">
        <span>Total subtitles: <strong id="subtitleCount">0</strong></span>
      </div>

      <div id="subtitleList"></div>
    </section>

    <section class="panel">
      <h2>4. Live Subtitle Preview</h2>

      <div class="preview">
        <div id="previewText">Aapke subtitles yahan dikhenge</div>
      </div>

      <label for="fontSize">Subtitle text size</label>
      <input id="fontSize" type="range" min="16" max="44" value="28">

      <label for="subtitleColor">Subtitle color</label>
      <select id="subtitleColor">
        <option value="#ffffff">White</option>
        <option value="#ffe08a">Golden</option>
        <option value="#a8e6cf">Mint</option>
        <option value="#ffb4a2">Warm pink</option>
      </select>

      <p class="hint">
        Preview audio ke current time ke hisaab se subtitle dikhata hai.
        Timing sahi rakhne ke liye har line ke start aur end time edit karein.
      </p>
    </section>

    <section class="panel">
      <h2>5. Export Karein</h2>

      <button id="srtBtn">Download SRT Subtitles</button>
      <button id="vttBtn">Download VTT Subtitles</button>
      <button id="txtBtn">Download Transcript TXT</button>
      <button id="jsonBtn">Download Project JSON</button>

      <p class="hint">
        SRT file ko CapCut, Premiere Pro aur doosre subtitle editors
        mein import kiya ja sakta hai. Platform compatibility alag ho sakti hai.
      </p>

      <p id="status" role="status" aria-live="polite"></p>
    </section>
  </main>

  <footer>
    Meditation Studio · Hindi Subtitle Editor
  </footer>

  <script>
    const $ = id => document.getElementById(id);

    const audio = $("audioPlayer");
    let subtitles = [];
    let nextId = 1;
    let audioUrl = null;
    let activeId = null;

    function timeLabel(seconds) {
      seconds = Math.max(0, Math.floor(Number(seconds) || 0));
      const m = Math.floor(seconds / 60);
      const s = seconds % 60;
      return String(m).padStart(2, "0") + ":" +
             String(s).padStart(2, "0");
    }

    function timestamp(seconds, format) {
      const ms = Math.max(0, Math.round((Number(seconds) || 0) * 1000));
      const h = Math.floor(ms / 3600000);
      const m = Math.floor((ms % 3600000) / 60000);
      const s = Math.floor((ms % 60000) / 1000);
      const milli = ms % 1000;

      const base = [h, m, s].map(v => String(v).padStart(2, "0")).join(":");
      return base + (format === "vtt" ? "." : ",") +
             String(milli).padStart(3, "0");
    }

    function notify(message) {
      $("status").textContent = message;
    }

    function getDuration() {
      return Number.isFinite(audio.duration) ? audio.duration : 0;
    }

    function makeSubtitle(text, start, end) {
      return {
        id: nextId++,
        text: text || "",
        start: Math.max(0, start),
        end: Math.max(start + 0.5, end)
      };
    }

    function sortSubtitles() {
      subtitles.sort((a, b) => a.start - b.start || a.id - b.id);
    }

    function render() {
      sortSubtitles();

      $("subtitleCount").textContent = subtitles.length;
      const list = $("subtitleList");
      list.replaceChildren();

      subtitles.forEach((sub, index) => {
        const card = document.createElement("div");
        card.className = "subtitle-row";
        card.dataset.id = sub.id;

        const heading = document.createElement("div");
        heading.className = "row-heading";

        const title = document.createElement("strong");
        title.textContent = "Subtitle " + (index + 1);

        const jump = document.createElement("button");
        jump.className = "secondary";
        jump.textContent = "▶ Is samay par";
        jump.type = "button";
        jump.addEventListener("click", () => {
          if (audio.src) {
            audio.currentTime = sub.start;
            audio.play().catch(() => {});
          } else {
            notify("Pehle audio upload karein.");
          }
        });

        heading.append(title, jump);
        card.append(heading);

        const timeGrid = document.createElement("div");
        timeGrid.className = "grid";

        const startLabel = document.createElement("label");
        startLabel.textContent = "Start (seconds)";
        const startInput = document.createElement("input");
        startInput.type = "number";
        startInput.min = "0";
        startInput.step = "0.1";
        startInput.value = sub.start;
        startLabel.append(startInput);

        const endLabel = document.createElement("label");
        endLabel.textContent = "End (seconds)";
        const endInput = document.createElement("input");
        endInput.type = "number";
        endInput.min = "0.1";
        endInput.step = "0.1";
        endInput.value = sub.end;
        endLabel.append(endInput);

        function updateTimes() {
          const start = Number(startInput.value);
          const end = Number(endInput.value);

          if (!Number.isFinite(start) || !Number.isFinite(end) ||
              start < 0 || end <= start) {
            notify("Sahi timing daalein: end time, start time se bada hona chahiye.");
            return;
          }

          sub.start = start;
          sub.end = end;
          notify("Subtitle timing update ho gayi.");
          updatePreview();
        }

        startInput.addEventListener("change", updateTimes);
        endInput.addEventListener("change", updateTimes);
        timeGrid.append(startLabel, endLabel);
        card.append(timeGrid);

        const textLabel = document.createElement("label");
        textLabel.textContent = "Hindi subtitle text";
        const textArea = document.createElement("textarea");
        textArea.rows = 3;
        textArea.value = sub.text;
        textArea.placeholder = "Yahan subtitle likhein...";
        textArea.addEventListener("input", () => {
          sub.text = textArea.value;
          updatePreview();
        });
        textLabel.append(textArea);
        card.append(textLabel);

        const actions = document.createElement("div");

        const insert = document.createElement("button");
        insert.className = "secondary";
        insert.textContent = "Iske baad jodein";
        insert.type = "button";
        insert.addEventListener("click", () => {
          const duration = Math.max(1, Number($("defaultDuration").value) || 4);
          const gap = Math.max(0, Number($("gapDuration").value) || 0);
          const start = sub.end + gap;
          subtitles.push(makeSubtitle("", start, start + duration));
          render();
        });

        const remove = document.createElement("button");
        remove.className = "danger";
        remove.textContent = "Delete";
        remove.type = "button";
        remove.addEventListener("click", () => {
          subtitles = subtitles.filter(item => item.id !== sub.id);
          render();
          updatePreview();
          notify("Subtitle delete ho gaya.");
        });

        actions.append(insert, remove);
        card.append(actions);
        list.append(card);
      });

      updatePreview();
    }

    function updatePreview() {
      const now = audio.currentTime || 0;
      const active = subtitles.find(s => now >= s.start && now < s.end);

      $("currentTime").textContent = timeLabel(now);
      $("duration").textContent = timeLabel(getDuration());

      const preview = $("previewText");
      preview.textContent = active ? active.text : "";
      preview.style.fontSize = $("fontSize").value + "px";
      preview.style.color = $("subtitleColor").value;

      if (!active) {
        preview.textContent = audio.src
          ? "Is samay koi subtitle nahi hai"
          : "Audio upload karein aur subtitles banayein";
      }

      if (activeId !== (active ? active.id : null)) {
        activeId = active ? active.id : null;
        document.querySelectorAll(".subtitle-row").forEach(card => {
          card.classList.toggle(
            "active",
            Number(card.dataset.id) === activeId
          );
        });
      }
    }

    function generateFromTranscript(append) {
      const lines = $("transcript").value
        .split(/\r?\n/)
        .map(line => line.trim())
        .filter(Boolean);

      if (!lines.length) {
        notify("Pehle transcript mein kuch text likhein ya paste karein.");
        return;
      }

      if (!append) subtitles = [];

      const duration = Math.max(1, Number($("defaultDuration").value) || 4);
      const gap = Math.max(0, Number($("gapDuration").value) || 0);

      let start = subtitles.length
        ? Math.max(...subtitles.map(s => s.end)) + gap
        : 0;

      lines.forEach(line => {
        subtitles.push(makeSubtitle(line, start, start + duration));
        start += duration + gap;
      });

      render();
      notify(lines.length + " subtitle lines add ho gayi hain. Timing review karein.");
    }

    function addSubtitle() {
      const duration = Math.max(1, Number($("defaultDuration").value) || 4);
      const gap = Math.max(0, Number($("gapDuration").value) || 0);
      const start = subtitles.length
        ? Math.max(...subtitles.map(s => s.end)) + gap
        : 0;

      subtitles.push(makeSubtitle("", start, start + duration));
      render();
      notify("Naya subtitle add ho gaya.");
    }

    function retime() {
      const duration = Math.max(1, Number($("defaultDuration").value) || 4);
      const gap = Math.max(0, Number($("gapDuration").value) || 0);
      let start = 0;

      subtitles.forEach(sub => {
        sub.start = start;
        sub.end = start + duration;
        start = sub.end + gap;
      });

      render();
      notify("Sab subtitles ki timing dobara set ho gayi.");
    }

    function download(filename, content, type) {
      const blob = new Blob([content], { type });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.href = url;
      link.download = filename;
      document.body.appendChild(link);
      link.click();
      link.remove();
      setTimeout(() => URL.revokeObjectURL(url), 1000);
    }

    function exportSrt() {
      if (!subtitles.length) {
        notify("Pehle subtitles banayein.");
        return;
      }

      const content = [...subtitles]
        .sort((a, b) => a.start - b.start)
        .map((s, i) =>
          (i + 1) + "\r\n" +
          timestamp(s.start, "srt") + " --> " +
          timestamp(s.end, "srt") + "\r\n" +
          s.text.trim()
        ).join("\r\n\r\n");

      download("hindi-subtitles.srt", "\uFEFF" + content + "\r\n",
        "application/x-subrip;charset=utf-8");
      notify("SRT subtitles download ho gaye.");
    }

    function exportVtt() {
      if (!subtitles.length) {
        notify("Pehle subtitles banayein.");
        return;
      }

      const content = "WEBVTT\r\n\r\n" + [...subtitles]
        .sort((a, b) => a.start - b.start)
        .map(s =>
          timestamp(s.start, "vtt") + " --> " +
          timestamp(s.end, "vtt") + "\r\n" +
          s.text.trim()
        ).join("\r\n\r\n");

      download("hindi-subtitles.vtt", "\uFEFF" + content + "\r\n",
        "text/vtt;charset=utf-8");
      notify("VTT subtitles download ho gaye.");
    }

    $("audioFile").addEventListener("change", function() {
      const file = this.files && this.files[0];
      if (!file) return;

      if (audioUrl) URL.revokeObjectURL(audioUrl);
      audioUrl = URL.createObjectURL(file);
      audio.src = audioUrl;

      audio.addEventListener("loadedmetadata", function onLoaded() {
        $("duration").textContent = timeLabel(getDuration());
        audio.removeEventListener("loadedmetadata", onLoaded);
        notify("Audio load ho gayi. Ab transcript paste karein.");
      });
    });

    audio.addEventListener("timeupdate", updatePreview);
    audio.addEventListener("seeked", updatePreview);
    $("fontSize").addEventListener("input", updatePreview);
    $("subtitleColor").addEventListener("change", updatePreview);

    $("generateBtn").addEventListener("click", () => generateFromTranscript(false));
    $("appendBtn").addEventListener("click", () => generateFromTranscript(true));
    $("addBtn").addEventListener("click", addSubtitle);
    $("retimeBtn").addEventListener("click", retime);

    $("clearBtn").addEventListener("click", () => {
      if (confirm("Kya aap saare subtitles delete karna chahte hain?")) {
        subtitles = [];
        render();
        notify("Saare subtitles delete ho gaye.");
      }
    });

    $("srtBtn").addEventListener("click", exportSrt);
    $("vttBtn").addEventListener("click", exportVtt);

    $("txtBtn").addEventListener("click", () => {
      if (!subtitles.length) {
        notify("Pehle subtitles banayein.");
        return;
      }
      download("hindi-transcript.txt",
        subtitles.map(s => s.text).join("\r\n") + "\r\n",
        "text/plain;charset=utf-8");
      notify("Transcript TXT download ho gaya.");
    });

    $("jsonBtn").addEventListener("click", () => {
      const project = {
        title: $("title")?.value || "Meditation Studio",
        audioFilename: $("audioFile").files[0]?.name || null,
        transcript: $("transcript").value,
        subtitles: subtitles.map(({ text, start, end }) => ({
          text, start, end
        }))
      };
      download("meditation-subtitle-project.json",
        JSON.stringify(project, null, 2),
        "application/json;charset=utf-8");
      notify("Project JSON download ho gaya.");
    });

    render();
  </script>
</body>
</html>
