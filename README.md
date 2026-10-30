# Stop-watch

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="theme-color" content="#0b1020">

    <title>Chrono | Stopwatch</title>

    <style>
        /* ---------- 1. Global Styles ---------- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --bg: #0b1020;
            --panel: #121a2e;
            --panel-light: #19233b;
            --text: #f8fafc;
            --muted: #94a3b8;
            --accent: #818cf8;
            --green: #34d399;
            --red: #fb7185;
            --border: rgba(148, 163, 184, 0.16);
        }

        body {
            min-height: 100vh;
            font-family: Inter, "Segoe UI", Arial, sans-serif;
            color: var(--text);
            background:
                radial-gradient(
                    circle at 50% 0%,
                    rgba(99, 102, 241, 0.18),
                    transparent 38%
                ),
                var(--bg);
            display: flex;
            flex-direction: column;
        }

        button {
            font: inherit;
        }

        button:focus-visible {
            outline: 3px solid #a5b4fc;
            outline-offset: 4px;
        }

        /* ---------- 2. Header ---------- */
        .header {
            width: min(1000px, 92%);
            margin: 0 auto;
            padding: 28px 0;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 1px solid var(--border);
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 1.4rem;
            font-weight: 800;
            letter-spacing: -0.5px;
        }

        .brand-icon {
            width: 42px;
            height: 42px;
            display: grid;
            place-items: center;
            border-radius: 13px;
            color: white;
            background: linear-gradient(135deg, #818cf8, #6366f1);
            font-size: 1.4rem;
        }

        .brand span {
            color: var(--accent);
        }

        .header-label {
            color: var(--muted);
            font-size: 0.85rem;
            letter-spacing: 1.5px;
            text-transform: uppercase;
        }

        /* ---------- 3. Main Layout ---------- */
        main {
            flex: 1;
            width: min(760px, 92%);
            margin: 0 auto;
            padding: 65px 0 55px;
        }

        .intro {
            text-align: center;
            margin-bottom: 35px;
        }

        .intro h1 {
            font-size: clamp(2rem, 5vw, 3rem);
            letter-spacing: -1.5px;
            margin-bottom: 12px;
        }

        .intro p {
            color: var(--muted);
            font-size: 1rem;
        }

        /* ---------- 4. Stopwatch Card ---------- */
        .stopwatch-card {
            background: linear-gradient(
                145deg,
                rgba(25, 35, 59, 0.98),
                rgba(18, 26, 46, 0.98)
            );
            border: 1px solid var(--border);
            border-radius: 28px;
            padding: 42px 35px;
            box-shadow: 0 25px 70px rgba(0, 0, 0, 0.28);
        }

        .timer-status {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 9px;
            color: var(--muted);
            font-size: 0.8rem;
            font-weight: 700;
            letter-spacing: 2px;
            text-transform: uppercase;
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #64748b;
        }

        .timer-status.running .status-dot {
            background: var(--green);
            box-shadow: 0 0 12px rgba(52, 211, 153, 0.7);
            animation: pulse 1.4s infinite;
        }

        .timer-status.paused .status-dot {
            background: #fbbf24;
        }

        @keyframes pulse {
            50% {
                opacity: 0.45;
            }
        }

        /* Large digital time display */
        .timer-display {
            display: flex;
            justify-content: center;
            align-items: baseline;
            gap: 3px;
            margin: 35px 0 12px;
            font-family: "Courier New", monospace;
            font-size: clamp(2.6rem, 9vw, 5.2rem);
            font-weight: 700;
            letter-spacing: -3px;
            font-variant-numeric: tabular-nums;
            white-space: nowrap;
        }

        .timer-display .milliseconds {
            color: var(--accent);
            font-size: 0.48em;
            letter-spacing: -1px;
            min-width: 2.1em;
        }

        .time-labels {
            display: flex;
            justify-content: center;
            gap: clamp(24px, 7vw, 65px);
            color: var(--muted);
            font-size: 0.7rem;
            letter-spacing: 1.8px;
            text-transform: uppercase;
            margin-bottom: 35px;
        }

        /* ---------- 5. Control Buttons ---------- */
        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            flex-wrap: wrap;
            gap: 14px;
        }

        .btn {
            min-width: 115px;
            padding: 14px 22px;
            border: 1px solid var(--border);
            border-radius: 13px;
            color: var(--text);
            background: var(--panel-light);
            cursor: pointer;
            font-weight: 700;
            transition:
                transform 0.2s ease,
                background 0.2s ease,
                box-shadow 0.2s ease,
                opacity 0.2s ease;
        }

        .btn:hover:not(:disabled) {
            transform: translateY(-3px);
        }

        .btn:active:not(:disabled) {
            transform: scale(0.97);
        }

        .btn:disabled {
            opacity: 0.35;
            cursor: not-allowed;
        }

        .btn-start {
            background: var(--green);
            border-color: var(--green);
            color: #052e25;
        }

        .btn-start:hover:not(:disabled) {
            background: #6ee7b7;
            box-shadow: 0 8px 25px rgba(52, 211, 153, 0.18);
        }

        .btn-pause {
            background: var(--red);
            border-color: var(--red);
            color: #4c0519;
        }

        .btn-pause:hover:not(:disabled) {
            background: #fda4af;
            box-shadow: 0 8px 25px rgba(251, 113, 133, 0.18);
        }

        .btn-lap:hover:not(:disabled),
        .btn-reset:hover:not(:disabled) {
            background: #293653;
        }

        /* ---------- 6. Lap History ---------- */
        .laps-section {
            margin-top: 32px;
            background: rgba(18, 26, 46, 0.85);
            border: 1px solid var(--border);
            border-radius: 22px;
            overflow: hidden;
        }

        .laps-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 23px 25px;
            border-bottom: 1px solid var(--border);
        }

        .laps-header h2 {
            font-size: 1.05rem;
        }

        .lap-count {
            color: var(--accent);
            background: rgba(129, 140, 248, 0.12);
            padding: 5px 11px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 700;
        }

        .table-wrap {
            width: 100%;
            overflow-x: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            text-align: left;
            font-variant-numeric: tabular-nums;
        }

        thead {
            background: rgba(255, 255, 255, 0.025);
        }

        th, td {
            padding: 16px 25px;
            border-bottom: 1px solid var(--border);
        }

        th {
            color: var(--muted);
            font-size: 0.72rem;
            letter-spacing: 1.2px;
            text-transform: uppercase;
            font-weight: 600;
        }

        td {
            font-family: "Courier New", monospace;
            font-size: 0.95rem;
        }

        td:first-child {
            font-family: Inter, "Segoe UI", Arial, sans-serif;
            color: var(--muted);
        }

        tbody tr {
            animation: appear 0.25s ease both;
            transition: background 0.2s ease;
        }

        tbody tr:hover {
            background: rgba(129, 140, 248, 0.06);
        }

        tbody tr:last-child td {
            border-bottom: none;
        }

        .fastest {
            color: var(--green);
        }

        .slowest {
            color: var(--red);
        }

        @keyframes appear {
            from {
                opacity: 0;
                transform: translateY(-6px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .empty-state {
            padding: 40px 20px;
            text-align: center;
            color: var(--muted);
            font-size: 0.9rem;
        }

        .empty-icon {
            display: block;
            font-size: 1.8rem;
            margin-bottom: 10px;
            opacity: 0.7;
        }

        /* ---------- 7. Footer ---------- */
        footer {
            text-align: center;
            color: var(--muted);
            font-size: 0.8rem;
            padding: 25px 15px;
            border-top: 1px solid var(--border);
        }

        /* ---------- 8. Responsive Layout ---------- */
        @media (max-width: 520px) {
            .header {
                padding: 20px 0;
            }

            .header-label {
                font-size: 0.68rem;
                letter-spacing: 1px;
            }

            main {
                padding-top: 45px;
            }

            .stopwatch-card {
                padding: 32px 14px;
                border-radius: 22px;
            }

            .timer-display {
                letter-spacing: -2px;
                gap: 1px;
                margin-top: 30px;
            }

            .time-labels {
                gap: 22px;
                font-size: 0.6rem;
                letter-spacing: 1px;
            }

            .controls {
                gap: 10px;
            }

            .btn {
                min-width: 0;
                flex: 1 1 40%;
                padding: 13px 10px;
            }

            .laps-header {
                padding: 20px 16px;
            }

            th, td {
                padding: 14px 13px;
                font-size: 0.8rem;
            }
        }

        @media (prefers-reduced-motion: reduce) {
            *, *::before, *::after {
                animation-duration: 0.01ms !important;
                transition-duration: 0.01ms !important;
            }
        }
    </style>
</head>

<body>

    <!-- ---------- Header ---------- -->
    <header class="header">
        <div class="brand">
            <div class="brand-icon" aria-hidden="true">◷</div>
            <div>chrono<span>.</span></div>
        </div>

        <div class="header-label">Precision Timer</div>
    </header>

    <main>
        <!-- ---------- Introduction ---------- -->
        <section class="intro">
            <h1>Every second counts.</h1>
            <p>Focus on the moment. Track every lap.</p>
        </section>

        <!-- ---------- Stopwatch ---------- -->
        <section class="stopwatch-card" aria-label="Stopwatch controls">

            <div class="timer-status" id="timerStatus">
                <span class="status-dot"></span>
                <span id="statusText">Ready to start</span>
            </div>

            <!-- Timer display -->
            <div
                class="timer-display"
                role="timer"
                aria-label="Elapsed time"
                aria-live="off">

                <span id="hours">00</span>:<span id="minutes">00</span>:<span id="seconds">00</span><span class="milliseconds">.<span id="milliseconds">000</span></span>
            </div>

            <div class="time-labels" aria-hidden="true">
                <span>Hours</span>
                <span>Minutes</span>
                <span>Seconds</span>
                <span>Millis</span>
            </div>

            <!-- Control buttons -->
            <div class="controls">
                <button class="btn btn-start" id="startBtn" type="button">
                    ▶ Start
                </button>

                <button class="btn btn-pause" id="pauseBtn" type="button" disabled>
                    Ⅱ Pause
                </button>

                <button class="btn btn-lap" id="lapBtn" type="button" disabled>
                    ⚑ Lap
                </button>

                <button class="btn btn-reset" id="resetBtn" type="button" disabled>
                    ↻ Reset
                </button>
            </div>
        </section>

        <!-- ---------- Lap History ---------- -->
        <section class="laps-section" aria-labelledby="lapsTitle">

            <div class="laps-header">
                <h2 id="lapsTitle">Lap History</h2>
                <span class="lap-count" id="lapCount">0 Laps</span>
            </div>

            <div class="table-wrap">
                <table>
                    <thead>
                        <tr>
                            <th scope="col">Lap</th>
                            <th scope="col">Lap Time</th>
                            <th scope="col">Total Time</th>
                        </tr>
                    </thead>

                    <tbody id="lapTableBody">
                        <!-- JavaScript inserts lap rows here -->
                    </tbody>
                </table>

                <div class="empty-state" id="emptyState">
                    <span class="empty-icon" aria-hidden="true">◷</span>
                    No laps recorded yet.
                    <br>
                    Start the timer and press Lap to record a split.
                </div>
            </div>
        </section>
    </main>

    <footer>
        <p>Made with HTML, CSS &amp; JavaScript · Keep moving forward.</p>
    </footer>

    <script>
        // ---------- 1. Get HTML elements ----------
        const hoursEl = document.getElementById("hours");
        const minutesEl = document.getElementById("minutes");
        const secondsEl = document.getElementById("seconds");
        const millisecondsEl = document.getElementById("milliseconds");

        const startBtn = document.getElementById("startBtn");
        const pauseBtn = document.getElementById("pauseBtn");
        const lapBtn = document.getElementById("lapBtn");
        const resetBtn = document.getElementById("resetBtn");

        const timerStatus = document.getElementById("timerStatus");
        const statusText = document.getElementById("statusText");

        const lapTableBody = document.getElementById("lapTableBody");
        const emptyState = document.getElementById("emptyState");
        const lapCount = document.getElementById("lapCount");

        // ---------- 2. Stopwatch state ----------
        let startTime = 0;
        let elapsedTime = 0;
        let animationFrameId = null;
        let isRunning = false;

        // Lap tracking state
        let previousLapTime = 0;
        let lapNumber = 0;
        let laps = [];

        // ---------- 3. Format elapsed time ----------
        // Input: elapsed milliseconds
        // Output: HH:MM:SS.mmm
        function formatTime(time) {
            const totalMilliseconds = Math.floor(time);

            const hours = Math.floor(
                totalMilliseconds / 3600000
            );

            const minutes = Math.floor(
                (totalMilliseconds % 3600000) / 60000
            );

            const seconds = Math.floor(
                (totalMilliseconds % 60000) / 1000
            );

            const milliseconds = totalMilliseconds % 1000;

            return (
                String(hours).padStart(2, "0") + ":" +
                String(minutes).padStart(2, "0") + ":" +
                String(seconds).padStart(2, "0") + "." +
                String(milliseconds).padStart(3, "0")
            );
        }

        // ---------- 4. Update the timer display ----------
        function updateDisplay(time) {
            const formatted = formatTime(time);
            const parts = formatted.split(/[:.]/);

            hoursEl.textContent = parts[0];
            minutesEl.textContent = parts[1];
            secondsEl.textContent = parts[2];
            millisecondsEl.textContent = parts[3];
        }

        // ---------- 5. Timer animation loop ----------
        // requestAnimationFrame updates the screen smoothly.
        // Actual elapsed time comes from performance.now(),
        // not from counting animation frames.
        function tick() {
            if (!isRunning) return;

            elapsedTime = performance.now() - startTime;

            updateDisplay(elapsedTime);

            animationFrameId = requestAnimationFrame(tick);
        }

        // ---------- 6. Start or resume the stopwatch ----------
        function startStopwatch() {
            if (isRunning) return;

            // Subtract previously accumulated time so Resume
            // continues from the paused value.
            startTime = performance.now() - elapsedTime;
            isRunning = true;

            timerStatus.classList.remove("paused");
            timerStatus.classList.add("running");
            statusText.textContent = "Stopwatch running";

            startBtn.disabled = true;
            pauseBtn.disabled = false;
            lapBtn.disabled = false;
            resetBtn.disabled = false;

            // Start a new animation loop.
            animationFrameId = requestAnimationFrame(tick);
        }

        // ---------- 7. Pause the stopwatch ----------
        function pauseStopwatch() {
            if (!isRunning) return;

            // Capture the latest elapsed time before stopping.
            elapsedTime = performance.now() - startTime;
            isRunning = false;

            cancelAnimationFrame(animationFrameId);
            animationFrameId = null;

            updateDisplay(elapsedTime);

            timerStatus.classList.remove("running");
            timerStatus.classList.add("paused");
            statusText.textContent = "Stopwatch paused";

            startBtn.disabled = false;
            startBtn.textContent = "▶ Resume";
            pauseBtn.disabled = true;
            lapBtn.disabled = true;
        }

        // ---------- 8. Record a lap ----------
        function recordLap() {
            if (!isRunning) return;

            const currentTime = performance.now() - startTime;

            // Lap time = current total time - previous lap time.
            const lapTime = currentTime - previousLapTime;

            lapNumber++;

            laps.push({
                number: lapNumber,
                lapTime: lapTime,
                totalTime: currentTime
            });

            previousLapTime = currentTime;

            renderLaps();
        }

        // ---------- 9. Display the lap history ----------
        function renderLaps() {
            lapTableBody.replaceChildren();
            emptyState.hidden = laps.length > 0;

            lapCount.textContent =
                laps.length + (laps.length === 1 ? " Lap" : " Laps");

            // Find fastest and slowest lap durations.
            const lapDurations = laps.map(lap => lap.lapTime);
            const fastestLap = Math.min(...lapDurations);
            const slowestLap = Math.max(...lapDurations);

            // Display newest laps first.
            [...laps].reverse().forEach(function (lap) {
                const row = document.createElement("tr");

                const numberCell = document.createElement("td");
                const lapTimeCell = document.createElement("td");
                const totalTimeCell = document.createElement("td");

                numberCell.textContent =
                    "Lap " + String(lap.number).padStart(2, "0");

                lapTimeCell.textContent = formatTime(lap.lapTime);
                totalTimeCell.textContent = formatTime(lap.totalTime);

                // Highlight fastest and slowest laps when they differ.
                if (fastestLap !== slowestLap) {
                    if (lap.lapTime === fastestLap) {
                        lapTimeCell.classList.add("fastest");
                        lapTimeCell.title = "Fastest lap";
                    } else if (lap.lapTime === slowestLap) {
                        lapTimeCell.classList.add("slowest");
                        lapTimeCell.title = "Slowest lap";
                    }
                }

                row.append(numberCell, lapTimeCell, totalTimeCell);
                lapTableBody.appendChild(row);
            });
        }

        // ---------- 10. Reset the stopwatch ----------
        function resetStopwatch() {
            isRunning = false;

            if (animationFrameId !== null) {
                cancelAnimationFrame(animationFrameId);
                animationFrameId = null;
            }

            startTime = 0;
            elapsedTime = 0;
            previousLapTime = 0;
            lapNumber = 0;
            laps = [];

            updateDisplay(0);
            renderLaps();

            timerStatus.classList.remove("running", "paused");
            statusText.textContent = "Ready to start";

            startBtn.disabled = false;
            startBtn.textContent = "▶ Start";
            pauseBtn.disabled = true;
            lapBtn.disabled = true;
            resetBtn.disabled = true;
        }

        // ---------- 11. Connect buttons to functions ----------
        startBtn.addEventListener("click", startStopwatch);
        pauseBtn.addEventListener("click", pauseStopwatch);
        lapBtn.addEventListener("click", recordLap);
        resetBtn.addEventListener("click", resetStopwatch);

        // Initialize the display on page load.
        updateDisplay(0);
        renderLaps();
    </script>

</body>
</html>
