<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Quiz – Electrónica de Potencia (28 preguntas)</title>
  <style>
    :root { --ok:#1a7f37; --bad:#b42318; --muted:#667085; --card:#f8fafc; --border:#e5e7eb; }
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; margin: 0; background:#fff; color:#111827; }
    header { padding: 20px; border-bottom: 1px solid var(--border); }
    main { max-width: 980px; margin: 0 auto; padding: 20px; }
    .hint { color: var(--muted); font-size: 0.95rem; margin-top: 6px; }
    .card { background: var(--card); border: 1px solid var(--border); border-radius: 14px; padding: 16px; margin: 14px 0; }
    .qtitle { font-weight: 800; margin: 0 0 10px; }
    .opts { display: grid; gap: 8px; }
    label { display:flex; gap:10px; align-items:flex-start; cursor:pointer; padding:10px; border:1px solid var(--border); border-radius:12px; background:#fff; }
    input[type="radio"] { margin-top: 2px; }
    .actions { display:flex; gap: 10px; flex-wrap: wrap; margin-top: 18px; position: sticky; bottom: 10px; background: rgba(255,255,255,0.92); padding: 10px; border: 1px solid var(--border); border-radius: 14px; backdrop-filter: blur(4px); }
    button {
      border: 1px solid var(--border);
      background: #111827; color:#fff;
      padding: 10px 14px; border-radius: 12px;
      cursor: pointer; font-weight: 800;
    }
    button.secondary { background:#fff; color:#111827; }
    #resultBox { border-radius: 14px; padding: 14px; border: 1px solid var(--border); background:#fff; margin-top: 18px; }
    .score { font-size: 1.15rem; font-weight: 900; margin: 0 0 6px; }
    .legend { color: var(--muted); margin: 0 0 10px; }
    .mark { font-weight: 900; }
    .ok { color: var(--ok); }
    .bad { color: var(--bad); }
    .explain { margin-top: 8px; font-size: 0.95rem; color:#111827; }
    .small { font-size: 0.9rem; color: var(--muted); }
    .pill { display:inline-block; padding: 2px 10px; border-radius: 999px; border: 1px solid var(--border); background:#fff; font-size: 0.85rem; color: var(--muted); }
    .topbar { display:flex; gap:10px; flex-wrap: wrap; align-items:center; }
    .counter { font-size: 0.95rem; color: var(--muted); }
    .grid2 { display:grid; gap:12px; grid-template-columns: 1fr; }
    @media (min-width: 900px){ .grid2{ grid-template-columns: 1fr 1fr; } }
  </style>
</head>
<body>
  <header>
    <div class="topbar">
      <h1 style="margin:0;">Quiz – Electrónica de Potencia</h1>
      <span class="pill">28 preguntas</span>
      <span class="counter" id="answeredCounter">Respondidas: 0/28</span>
    </div>
    <div class="hint">
      Marca una opción por pregunta y presiona <b>Calificar</b>.
      Al final verás tu puntaje y qué preguntas están bien o mal (con explicación).
      Incluye preguntas de rectificación (Vmax=220 V, f=60 Hz, R=50 Ω) y teoría (disparo, SCR/TRIAC, control, conmutación).
    </div>
  </header>

  <main>
    <form id="quizForm" autocomplete="off">
      <div class="grid2" id="questions"></div>

      <div class="actions">
        <button type="button" id="gradeBtn">Calificar</button>
        <button type="button" class="secondary" id="resetBtn">Reiniciar</button>
        <button type="button" class="secondary" id="scrollResultBtn">Ir al resultado</button>
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
    // Banco de 28 preguntas (8 anteriores + 20 nuevas)
    const BANK = [
      {
        id:"q1",
        title:"¿Qué significa “disparo” en dispositivos de potencia (ej. SCR/TRIAC)?",
        options:{
          a:"Aplicar una señal de control para pasar del estado de bloqueo (OFF) a conducción (ON).",
          b:"Aumentar el voltaje de entrada hasta romper el dispositivo intencionalmente.",
          c:"Conectar un capacitor en paralelo para reducir el rizado.",
          d:"Cambiar la frecuencia de la red para encender el semiconductor."
        },
        correct:"a",
        why:"Disparo = señal a la compuerta/gate para encender el dispositivo (de bloqueo a conducción)."
      },
      {
        id:"q2",
        title:"Diferencia clave entre SCR y TRIAC:",
        options:{
          a:"El SCR es bidireccional y el TRIAC es unidireccional.",
          b:"El SCR conduce en un sentido; el TRIAC conduce en ambos sentidos.",
          c:"Ambos son exactamente iguales; solo cambia el nombre comercial.",
          d:"El TRIAC solo sirve para DC y el SCR solo para AC."
        },
        correct:"b",
        why:"SCR: unidireccional. TRIAC: bidireccional (control directo de AC)."
      },
      {
        id:"q3",
        title:"¿Qué es un rectificador no controlado?",
        options:{
          a:"Convierte AC a DC usando diodos; la conducción depende de la polaridad de la fuente.",
          b:"Convierte DC a AC usando transistores y modulación PWM.",
          c:"Convierte AC a AC cambiando la frecuencia con un filtro RC.",
          d:"Convierte DC a DC usando solo resistencias."
        },
        correct:"a",
        why:"No controlado: diodos; rectifica AC→DC sin regular por ángulo de disparo."
      },
      {
        id:"q4",
        title:"¿Cómo se regula la salida en un rectificador controlado con SCR?",
        options:{
          a:"Variando el ángulo de disparo (α).",
          b:"Solo cambiando la resistencia de carga.",
          c:"Solo cambiando la frecuencia de la red.",
          d:"No se puede regular; siempre entrega el mismo DC."
        },
        correct:"a",
        why:"Rectificador controlado: se ajusta el instante de conducción mediante α."
      },
      {
        id:"q5",
        title:"En electrónica de potencia, ¿qué variable es típica para controlar convertidores DC-DC tipo buck/boost?",
        options:{
          a:"Ciclo de trabajo (duty cycle) de la PWM.",
          b:"Color del disipador térmico.",
          c:"Longitud del cable de alimentación.",
          d:"Altitud del lugar de instalación."
        },
        correct:"a",
        why:"En DC-DC el control más común es el duty cycle (D) de la PWM."
      },
      {
        id:"q6",
        title:"Rectificador con transformador con toma media (2 diodos): si f=60 Hz, ¿frecuencia de la señal rectificada en la carga?",
        options:{ a:"60 Hz", b:"120 Hz", c:"30 Hz", d:"240 Hz" },
        correct:"b",
        why:"Es onda completa: el rizado se duplica → 2f = 120 Hz."
      },
      {
        id:"q7",
        title:"Para una senoidal con Vmax=220 V, ¿cuál es el valor eficaz (RMS) de esa senoidal?",
        options:{ a:"220 V", b:"155.6 V", c:"311 V", d:"110 V" },
        correct:"b",
        why:"Vrms = Vmax/√2 = 220/1.414 ≈ 155.6 V."
      },
      {
        id:"q8",
        title:"¿Por qué la electrónica de potencia suele operar en conmutación (ON/OFF) y no linealmente?",
        options:{
          a:"Para minimizar pérdidas y mejorar eficiencia (menos disipación en calor).",
          b:"Porque así se elimina toda la interferencia electromagnética.",
          c:"Porque en conmutación la corriente siempre es cero.",
          d:"Porque no se necesitan protecciones eléctricas."
        },
        correct:"a",
        why:"En ON/OFF se reducen pérdidas promedio y calor comparado con operación lineal."
      },

      // 20 nuevas (q9-q28)
      {
        id:"q9",
        title:"En un SCR, después de dispararse (encender), ¿cuándo se apaga normalmente?",
        options:{
          a:"Cuando se retira inmediatamente la señal de compuerta.",
          b:"Cuando la corriente cae por debajo de la corriente de mantenimiento (o se hace cero en AC).",
          c:"Solo si sube la temperatura del disipador.",
          d:"Cuando la frecuencia sube por encima de 1 kHz."
        },
        correct:"b",
        why:"El SCR queda enclavado: se apaga cuando la corriente cae bajo la corriente de mantenimiento."
      },
      {
        id:"q10",
        title:"¿Cuál de estos es un uso típico del TRIAC?",
        options:{
          a:"Rectificación AC→DC en una fuente conmutada de alta frecuencia.",
          b:"Control de potencia en AC por ángulo de fase (dimmer, calefacción).",
          c:"Conversión DC→DC con inductor (buck).",
          d:"Medición de señales en instrumentación."
        },
        correct:"b",
        why:"TRIAC se usa mucho en control directo de AC (fase) como dimmers y calefacción."
      },
      {
        id:"q11",
        title:"¿Qué significa que un diodo sea “ideal” en problemas de rectificadores?",
        options:{
          a:"Tiene caída de voltaje cero en conducción y bloqueo perfecto en inversa.",
          b:"Tiene caída fija de 0.7 V siempre.",
          c:"Siempre conduce en ambos sentidos.",
          d:"Tiene resistencia infinita en directa."
        },
        correct:"a",
        why:"Ideal: Vd≈0 en directa y no conduce en inversa."
      },
      {
        id:"q12",
        title:"En un rectificador de onda completa (toma media o puente), el rizado (sin filtro) respecto a la red es:",
        options:{ a:"Igual a f", b:"El doble (2f)", c:"La mitad (f/2)", d:"Cuatro veces (4f)" },
        correct:"b",
        why:"Onda completa produce dos semiciclos positivos por período → 2f."
      },
      {
        id:"q13",
        title:"¿Qué se entiende por “carga resistiva” (R pura) en un rectificador?",
        options:{
          a:"La corriente se adelanta al voltaje.",
          b:"La corriente se atrasa al voltaje.",
          c:"La corriente está en fase con el voltaje (i = v/R).",
          d:"No circula corriente."
        },
        correct:"c",
        why:"Con R pura, i(t)=v(t)/R y están en fase."
      },
      {
        id:"q14",
        title:"En un control por ángulo de fase con SCR en AC, aumentar α (disparar más tarde) generalmente:",
        options:{
          a:"Aumenta el voltaje promedio entregado a la carga.",
          b:"Disminuye el voltaje promedio entregado a la carga.",
          c:"Duplica la frecuencia de salida.",
          d:"Elimina completamente armónicos."
        },
        correct:"b",
        why:"Mientras más tarde conduces, menor área bajo la curva → menor valor promedio/eficaz entregado."
      },
      {
        id:"q15",
        title:"¿Qué variable describe mejor la “eficiencia” η de un convertidor?",
        options:{
          a:"η = P_salida / P_entrada",
          b:"η = Vmax / Vrms",
          c:"η = I_pico / I_rms",
          d:"η = f_salida / f_entrada"
        },
        correct:"a",
        why:"Eficiencia: potencia útil de salida sobre potencia de entrada."
      },
      {
        id:"q16",
        title:"¿Cuál es un motivo típico para incluir disipador de calor en semiconductores de potencia?",
        options:{
          a:"Porque el disipador aumenta la tensión de ruptura.",
          b:"Porque reduce pérdidas de conmutación a cero.",
          c:"Porque ayuda a evacuar calor por pérdidas de conducción y conmutación.",
          d:"Porque hace que el dispositivo conduzca en ambos sentidos."
        },
        correct:"c",
        why:"Se disipa calor generado por pérdidas → mantener temperatura segura."
      },
      {
        id:"q17",
        title:"¿Qué es dv/dt en electrónica de potencia?",
        options:{
          a:"La rapidez de cambio de voltaje en el tiempo.",
          b:"La energía consumida por hora.",
          c:"La resistencia equivalente en DC.",
          d:"La corriente promedio de carga."
        },
        correct:"a",
        why:"dv/dt es la pendiente de voltaje; afecta esfuerzos, EMI y disparos no deseados."
      },
      {
        id:"q18",
        title:"En un rectificador con filtro capacitivo, el capacitor tiende a:",
        options:{
          a:"Aumentar el rizado de salida.",
          b:"Reducir el rizado almacenando energía y suavizando el voltaje.",
          c:"Convertir DC en AC.",
          d:"Duplicar la frecuencia de la red."
        },
        correct:"b",
        why:"El capacitor se carga en picos y entrega energía entre picos → menos rizado."
      },
      {
        id:"q19",
        title:"¿Cuál es una variable típica de calidad de energía relacionada con armónicos?",
        options:{ a:"THD", b:"RPM", c:"SOC", d:"Lux" },
        correct:"a",
        why:"THD (Total Harmonic Distortion) mide distorsión armónica."
      },
      {
        id:"q20",
        title:"¿Qué describe mejor “conmutación” en electrónica de potencia?",
        options:{
          a:"Operación del semiconductor en región lineal para amplificar señales.",
          b:"Encendido/apagado rápido de dispositivos para controlar potencia.",
          c:"Cambiar el material del semiconductor durante operación.",
          d:"Aumentar la resistencia para bajar corriente."
        },
        correct:"b",
        why:"Conmutación = switching ON/OFF para controlar energía con alta eficiencia."
      },
      {
        id:"q21",
        title:"En un convertidor DC-DC tipo buck ideal, el voltaje de salida promedio se relaciona con el duty cycle como:",
        options:{
          a:"Vout = Vin / D",
          b:"Vout = Vin · D",
          c:"Vout = Vin · (1-D)",
          d:"Vout = Vin · D²"
        },
        correct:"b",
        why:"Buck ideal: Vout = D·Vin."
      },
      {
        id:"q22",
        title:"¿Cuál de estos es un riesgo típico de diseño en puentes conmutados que el “dead-time” ayuda a evitar?",
        options:{
          a:"Rizado excesivo en el capacitor.",
          b:"Shoot-through (cortocircuito momentáneo por transistores superior e inferior encendidos a la vez).",
          c:"Pérdidas cero en conducción.",
          d:"Aumento del factor de potencia a 1."
        },
        correct:"b",
        why:"Dead-time evita encendidos simultáneos en una rama del puente (shoot-through)."
      },
      {
        id:"q23",
        title:"En un rectificador con carga R=50 Ω y una senoidal de Vmax=220 V (ideal), ¿cuál es la corriente pico en la carga (si se aplica directamente esa senoidal a R)?",
        options:{
          a:"4.4 A",
          b:"2.2 A",
          c:"1.1 A",
          d:"0.5 A"
        },
        correct:"a",
        why:"Ipico = Vpico/R = 220/50 = 4.4 A."
      },
      {
        id:"q24",
        title:"¿Qué es el “factor de potencia” (FP) en sistemas AC?",
        options:{
          a:"La relación entre potencia activa (P) y potencia aparente (S).",
          b:"La relación entre Vmax y Vrms.",
          c:"La relación entre la frecuencia y el duty cycle.",
          d:"La cantidad de armónicos de voltaje."
        },
        correct:"a",
        why:"FP = P/S (en sinusoidal también = cos φ)."
      },
      {
        id:"q25",
        title:"¿Cuál es una diferencia práctica común entre rectificador de toma media y rectificador en puente (para onda completa)?",
        options:{
          a:"El de toma media usa 2 diodos pero requiere transformador con derivación central.",
          b:"El de puente usa 2 diodos y requiere toma media.",
          c:"Ambos siempre usan 4 diodos.",
          d:"Ninguno puede entregar DC."
        },
        correct:"a",
        why:"Toma media: 2 diodos + trafo con toma. Puente: 4 diodos sin toma."
      },
      {
        id:"q26",
        title:"En un TRIAC, las terminales principales se llaman:",
        options:{ a:"A y K", b:"MT1 y MT2", c:"E y B", d:"C y E" },
        correct:"b",
        why:"TRIAC: MT1, MT2 y compuerta (G)."
      },
      {
        id:"q27",
        title:"¿Qué representa “di/dt” en conmutación?",
        options:{
          a:"La rapidez de cambio de corriente en el tiempo.",
          b:"La variación de potencia en función del duty.",
          c:"La tensión máxima del transformador.",
          d:"La resistencia térmica del disipador."
        },
        correct:"a",
        why:"di/dt es la pendiente de corriente; afecta esfuerzos y EMI."
      },
      {
        id:"q28",
        title:"¿Qué afirmación es correcta sobre rectificación (sin filtro) y la salida DC?",
        options:{
          a:"La salida es perfectamente constante (sin rizado).",
          b:"La salida tiene componente DC y componente AC (rizado).",
          c:"La salida solo tiene componente AC.",
          d:"La salida siempre es negativa."
        },
        correct:"b",
        why:"Rectificación produce un valor promedio (DC) pero mantiene ondulación (AC) si no hay filtro."
      }
    ];

    const form = document.getElementById("quizForm");
    const questionsWrap = document.getElementById("questions");
    const gradeBtn = document.getElementById("gradeBtn");
    const resetBtn = document.getElementById("resetBtn");
    const scrollResultBtn = document.getElementById("scrollResultBtn");
    const scoreText = document.getElementById("scoreText");
    const details = document.getElementById("details");
    const answeredCounter = document.getElementById("answeredCounter");

    function renderQuestions(){
      questionsWrap.innerHTML = BANK.map((q, idx) => {
        const num = idx + 1;
        const optsHtml = Object.entries(q.options).map(([k, txt]) => `
          <label>
            <input type="radio" name="${q.id}" value="${k}">
            <span><b>${k.toUpperCase()})</b> ${txt}</span>
          </label>
        `).join("");

        return `
          <section class="card" data-q="${q.id}">
            <p class="qtitle">${num}) ${q.title}</p>
            <div class="opts">${optsHtml}</div>
            <div class="explain small" data-explain></div>
          </section>
        `;
      }).join("");
    }

    function getSelected(qid){
      const el = form.querySelector(`input[name="${qid}"]:checked`);
      return el ? el.value : null;
    }

    function updateAnsweredCounter(){
      let answered = 0;
      for (const q of BANK){
        if (getSelected(q.id)) answered++;
      }
      answeredCounter.textContent = `Respondidas: ${answered}/${BANK.length}`;
    }

    function clearMarks(){
      form.querySelectorAll("[data-explain]").forEach(x => x.textContent = "");
      form.querySelectorAll("section.card label").forEach(l => {
        l.style.borderColor = "var(--border)";
        l.style.background = "#fff";
      });
      scoreText.textContent = "—";
      details.innerHTML = "";
    }

    function grade(){
      let correctCount = 0;
      const detailLines = [];

      // Limpia estilos previos
      form.querySelectorAll("[data-explain]").forEach(x => x.textContent = "");
      form.querySelectorAll("section.card label").forEach(l => {
        l.style.borderColor = "var(--border)";
        l.style.background = "#fff";
      });

      for (let i=0; i<BANK.length; i++){
        const q = BANK[i];
        const selected = getSelected(q.id);
        const card = form.querySelector(`section.card[data-q="${q.id}"]`);
        const explainBox = card.querySelector("[data-explain]");

        // Marca opciones
        const labels = card.querySelectorAll("label");
        labels.forEach(label => {
          const input = label.querySelector("input");
          if (!input) return;

          if (input.value === q.correct){
            label.style.borderColor = "rgba(26,127,55,0.35)";
            label.style.background = "rgba(26,127,55,0.06)";
          }
          if (selected && input.value === selected && selected !== q.correct){
            label.style.borderColor = "rgba(180,35,24,0.35)";
            label.style.background = "rgba(180,35,24,0.06)";
          }
        });

        const isCorrect = selected === q.correct;
        if (isCorrect) correctCount++;

        const mark = isCorrect ? `<span class="mark ok">✔</span>` : `<span class="mark bad">✘</span>`;
        const selText = selected ? selected.toUpperCase() : "—";
        const corText = q.correct.toUpperCase();

        explainBox.innerHTML = `${mark} Tu respuesta: <b>${selText}</b> · Correcta: <b>${corText}</b><br><span class="small">${q.why}</span>`;

        detailLines.push(`
          <div style="margin:10px 0; padding:10px; border:1px solid var(--border); border-radius:12px;">
            <div><b>${mark} Pregunta ${i+1}</b> · Tu respuesta: <b>${selText}</b> · Correcta: <b>${corText}</b></div>
            <div class="small" style="margin-top:6px;">${q.why}</div>
          </div>
        `);
      }

      const pct = Math.round((correctCount / BANK.length) * 100);
      scoreText.textContent = `${correctCount}/${BANK.length} (${pct}%)`;
      details.innerHTML = detailLines.join("");

      document.getElementById("resultBox").scrollIntoView({ behavior:"smooth", block:"start" });
    }

    function resetAll(){
      form.reset();
      clearMarks();
      updateAnsweredCounter();
      window.scrollTo({ top: 0, behavior: "smooth" });
    }

    // init
    renderQuestions();
    updateAnsweredCounter();

    // listeners
    form.addEventListener("change", (e) => {
      if (e.target && e.target.matches('input[type="radio"]')){
        updateAnsweredCounter();
      }
    });

    gradeBtn.addEventListener("click", grade);
    resetBtn.addEventListener("click", resetAll);
    scrollResultBtn.addEventListener("click", () => {
      document.getElementById("resultBox").scrollIntoView({ behavior:"smooth", block:"start" });
    });
  </script>
</body>
</html>
