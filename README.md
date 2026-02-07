<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Quiz – Electrónica de Potencia</title>
  <style>
    :root { --ok:#1a7f37; --bad:#b42318; --muted:#667085; --card:#f8fafc; --border:#e5e7eb; }
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; margin: 0; background:#fff; color:#111827; }
    header { padding: 20px; border-bottom: 1px solid var(--border); }
    main { max-width: 920px; margin: 0 auto; padding: 20px; }
    .hint { color: var(--muted); font-size: 0.95rem; margin-top: 6px; }
    .card { background: var(--card); border: 1px solid var(--border); border-radius: 14px; padding: 16px; margin: 14px 0; }
    .qtitle { font-weight: 700; margin: 0 0 10px; }
    .opts { display: grid; gap: 8px; }
    label { display:flex; gap:10px; align-items:flex-start; cursor:pointer; padding:10px; border:1px solid var(--border); border-radius:12px; background:#fff; }
    input[type="radio"] { margin-top: 2px; }
    .actions { display:flex; gap: 10px; flex-wrap: wrap; margin-top: 18px; }
    button {
      border: 1px solid var(--border);
      background: #111827; color:#fff;
      padding: 10px 14px; border-radius: 12px;
      cursor: pointer; font-weight: 700;
    }
    button.secondary { background:#fff; color:#111827; }
    button:disabled { opacity: 0.6; cursor: not-allowed; }
    #resultBox { border-radius: 14px; padding: 14px; border: 1px solid var(--border); background:#fff; margin-top: 18px; }
    .score { font-size: 1.15rem; font-weight: 800; margin: 0 0 6px; }
    .legend { color: var(--muted); margin: 0 0 10px; }
    .mark { font-weight: 800; }
    .ok { color: var(--ok); }
    .bad { color: var(--bad); }
    .explain { margin-top: 8px; font-size: 0.95rem; color:#111827; }
    .small { font-size: 0.9rem; color: var(--muted); }
    .pill { display:inline-block; padding: 2px 10px; border-radius: 999px; border: 1px solid var(--border); background:#fff; font-size: 0.85rem; color: var(--muted); }
    .grid2 { display:grid; gap:12px; grid-template-columns: 1fr; }
    @media (min-width: 900px){ .grid2{ grid-template-columns: 1fr 1fr; } }
  </style>
</head>
<body>
  <header>
    <h1 style="margin:0;">Quiz – Electrónica de Potencia</h1>
    <div class="hint">
      Marca una opción por pregunta y presiona <b>Calificar</b>. Al final verás tu puntaje y qué está bien o mal.
      Incluye una pregunta del ejercicio del rectificador (Vmax=220 V, f=60 Hz, R=50 Ω).
    </div>
  </header>

  <main>
    <form id="quizForm" autocomplete="off">
      <div class="grid2">
        <!-- Q1 -->
        <section class="card" data-q="q1">
          <p class="qtitle">1) ¿Qué significa “disparo” en dispositivos de potencia (ej. SCR/TRIAC)?</p>
          <div class="opts">
            <label><input type="radio" name="q1" value="a">Aplicar una señal de control para pasar del estado de bloqueo (OFF) a conducción (ON).</label>
            <label><input type="radio" name="q1" value="b">Aumentar el voltaje de entrada hasta romper el dispositivo intencionalmente.</label>
            <label><input type="radio" name="q1" value="c">Conectar un capacitor en paralelo para reducir el rizado.</label>
            <label><input type="radio" name="q1" value="d">Cambiar la frecuencia de la red para encender el semiconductor.</label>
          </div>
          <div class="explain small" data-explain></div>
        </section>

        <!-- Q2 -->
        <section class="card" data-q="q2">
          <p class="qtitle">2) Diferencia clave entre SCR y TRIAC:</p>
          <div class="opts">
            <label><input type="radio" name="q2" value="a">El SCR es bidireccional y el TRIAC es unidireccional.</label>
            <label><input type="radio" name="q2" value="b">El SCR conduce en un sentido; el TRIAC conduce en ambos sentidos.</label>
            <label><input type="radio" name="q2" value="c">Ambos son exactamente iguales; solo cambia el nombre comercial.</label>
            <label><input type="radio" name="q2" value="d">El TRIAC solo sirve para DC y el SCR solo para AC.</label>
          </div>
          <div class="explain small" data-explain></div>
        </section>

        <!-- Q3 -->
        <section class="card" data-q="q3">
          <p class="qtitle">3) ¿Qué es un rectificador no controlado?</p>
          <div class="opts">
            <label><input type="radio" name="q3" value="a">Convierte AC a DC usando diodos; la conducción depende de la polaridad de la fuente.</label>
            <label><input type="radio" name="q3" value="b">Convierte DC a AC usando transistores y modulación PWM.</label>
            <label><input type="radio" name="q3" value="c">Convierte AC a AC cambiando la frecuencia con un filtro RC.</label>
            <label><input type="radio" name="q3" value="d">Convierte DC a DC usando solo resistencias.</label>
          </div>
          <div class="explain small" data-explain></div>
        </section>

        <!-- Q4 -->
        <section class="card" data-q="q4">
          <p class="qtitle">4) ¿Cómo se regula la salida en un rectificador controlado con SCR?</p>
          <div class="opts">
            <label><input type="radio" name="q4" value="a">Variando el ángulo de disparo (α).</label>
            <label><input type="radio" name="q4" value="b">Solo cambiando la resistencia de carga.</label>
            <label><input type="radio" name="q4" value="c">Solo cambiando la frecuencia de la red.</label>
            <label><input type="radio" name="q4" value="d">No se puede regular; siempre entrega el mismo DC.</label>
          </div>
          <div class="explain small" data-explain></div>
        </section>

        <!-- Q5 -->
        <section class="card" data-q="q5">
          <p class="qtitle">5) En electrónica de potencia, ¿qué variable es típica para controlar convertidores DC-DC tipo buck/boost?</p>
          <div class="opts">
            <label><input type="radio" name="q5" value="a">Ciclo de trabajo (duty cycle) de la PWM.</label>
            <label><input type="radio" name="q5" value="b">Color del disipador térmico.</label>
            <label><input type="radio" name="q5" value="c">Longitud del cable de alimentación.</label>
            <label><input type="radio" name="q5" value="d">Altitud del lugar de instalación.</label>
          </div>
          <div class="explain small" data-explain></div>
        </section>

        <!-- Q6 -->
        <section class="card" data-q="q6">
          <p class="qtitle">6) Ejercicio (concepto): Rectificador bifásico de media onda con transformador con toma media (2 diodos ideales). Si V<sub>max</sub>=220 V y f=60 Hz, ¿cuál es la frecuencia de la señal rectificada en la carga?</p>
          <div class="opts">
            <label><input type="radio" name="q6" value="a">60 Hz</label>
            <label><input type="radio" name="q6" value="b">120 Hz</label>
            <label><input type="radio" name="q6" value="c">30 Hz</label>
            <label><input type="radio" name="q6" value="d">240 Hz</label>
          </div>
          <div class="explain small" data-explain></div>
        </section>

        <!-- Q7 -->
        <section class="card" data-q="q7">
          <p class="qtitle">7) Ejercicio (concepto): Con V<sub>max</sub>=220 V (onda senoidal), ¿cuál es el valor eficaz (RMS) de esa tensión senoidal?</p>
          <div class="opts">
            <label><input type="radio" name="q7" value="a">220 V</label>
            <label><input type="radio" name="q7" value="b">155.6 V</label>
            <label><input type="radio" name="q7" value="c">311 V</label>
            <label><input type="radio" name="q7" value="d">110 V</label>
          </div>
          <div class="explain small" data-explain></div>
        </section>

        <!-- Q8 -->
        <section class="card" data-q="q8">
          <p class="qtitle">8) ¿Cuál es la razón por la que la electrónica de potencia suele operar en conmutación (ON/OFF) y no linealmente?</p>
          <div class="opts">
            <label><input type="radio" name="q8" value="a">Para minimizar pérdidas y mejorar eficiencia (menos disipación en calor).</label>
            <label><input type="radio" name="q8" value="b">Porque así se elimina toda la interferencia electromagnética.</label>
            <label><input type="radio" name="q8" value="c">Porque en conmutación la corriente siempre es cero.</label>
            <label><input type="radio" name="q8" value="d">Porque no se necesitan protecciones eléctricas.</label>
          </div>
          <div class="explain small" data-explain></div>
        </section>
      </div>

      <div class="actions">
        <button type="button" id="gradeBtn">Calificar</button>
        <button type="button" class="secondary" id="resetBtn">Reiniciar</button>
      </div>

      <div id="resultBox" aria-live="polite">
        <p class="score">Resultado: <span id="scoreText">—</span></p>
        <p class="legend">
          <span class="pill"><span class="mark ok">✔</span> correcta</span>
          <span class="pill"><span class="mark bad">✘</span> incorrecta</span>
          <span class="pill">Se muestra la explicación al calificar</span>
        </p>
        <div id="details"></div>
      </div>
    </form>
  </main>

  <script>
    // Clave de respuestas + explicación corta
    const ANSWERS = {
      q1: { correct: "a", why: "Disparo = señal de control (gate/compuerta) para encender el dispositivo (pasar de bloqueo a conducción)." },
      q2: { correct: "b", why: "SCR: unidireccional. TRIAC: bidireccional (ideal para control directo de AC)." },
      q3: { correct: "a", why: "No controlado: diodos; rectifica AC→DC sin control por ángulo de disparo." },
      q4: { correct: "a", why: "Controlado con SCR: se regula la salida variando el ángulo de disparo (α)." },
      q5: { correct: "a", why: "En DC-DC, la variable típica de control es el duty cycle (D) de la PWM." },
      q6: { correct: "b", why: "Rectificador de onda completa (toma media, 2 diodos): la frecuencia del rizado se duplica: 2f = 120 Hz." },
      q7: { correct: "b", why: "Para una senoidal: Vrms = Vmax/√2 = 220/1.414 ≈ 155.6 V." },
      q8: { correct: "a", why: "En ON/OFF las pérdidas promedio son menores → mayor eficiencia y menos calor que operación lineal." }
    };

    const form = document.getElementById("quizForm");
    const gradeBtn = document.getElementById("gradeBtn");
    const resetBtn = document.getElementById("resetBtn");
    const scoreText = document.getElementById("scoreText");
    const details = document.getElementById("details");

    function getSelected(name){
      const el = form.querySelector(`input[name="${name}"]:checked`);
      return el ? el.value : null;
    }

    function grade(){
      let total = 0;
      let correctCount = 0;
      const detailLines = [];

      // Limpia estilos previos
      form.querySelectorAll("[data-explain]").forEach(x => x.textContent = "");
      form.querySelectorAll("section.card label").forEach(l => {
        l.style.borderColor = "var(--border)";
        l.style.background = "#fff";
      });

      for (const qid of Object.keys(ANSWERS)){
        total++;
        const selected = getSelected(qid);
        const { correct, why } = ANSWERS[qid];
        const card = form.querySelector(`section.card[data-q="${qid}"]`);
        const explainBox = card.querySelector("[data-explain]");

        // Marca opciones
        const labels = card.querySelectorAll("label");
        labels.forEach(label => {
          const input = label.querySelector("input");
          if (!input) return;

          if (input.value === correct){
            // Respuesta correcta (marcar en verde suave)
            label.style.borderColor = "rgba(26,127,55,0.35)";
            label.style.background = "rgba(26,127,55,0.06)";
          }
          if (selected && input.value === selected && selected !== correct){
            // Seleccionada incorrecta (rojo suave)
            label.style.borderColor = "rgba(180,35,24,0.35)";
            label.style.background = "rgba(180,35,24,0.06)";
          }
        });

        let isCorrect = selected === correct;
        if (isCorrect) correctCount++;

        // Explicación y detalle
        const mark = isCorrect ? `<span class="mark ok">✔</span>` : `<span class="mark bad">✘</span>`;
        const selText = selected ? selected.toUpperCase() : "—";
        const corText = correct.toUpperCase();

        explainBox.innerHTML = `${mark} Tu respuesta: <b>${selText}</b> · Correcta: <b>${corText}</b><br><span class="small">${why}</span>`;

        detailLines.push(`
          <div style="margin:10px 0; padding:10px; border:1px solid var(--border); border-radius:12px;">
            <div><b>${mark} ${qid.toUpperCase()}</b> · Tu respuesta: <b>${selText}</b> · Correcta: <b>${corText}</b></div>
            <div class="small" style="margin-top:6px;">${why}</div>
          </div>
        `);
      }

      const pct = Math.round((correctCount / total) * 100);
      scoreText.textContent = `${correctCount}/${total} (${pct}%)`;
      details.innerHTML = detailLines.join("");

      // Desplaza a resultados
      document.getElementById("resultBox").scrollIntoView({ behavior:"smooth", block:"start" });
    }

    function resetAll(){
      form.reset();
      scoreText.textContent = "—";
      details.innerHTML = "";
      form.querySelectorAll("[data-explain]").forEach(x => x.textContent = "");
      form.querySelectorAll("section.card label").forEach(l => {
        l.style.borderColor = "var(--border)";
        l.style.background = "#fff";
      });
      window.scrollTo({ top: 0, behavior: "smooth" });
    }

    gradeBtn.addEventListener("click", grade);
    resetBtn.addEventListener("click", resetAll);
  </script>
</body>
</html>
