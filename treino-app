import { useState, useEffect, useMemo, useRef } from "react";
import {
  Home, Dumbbell, BookOpen, TrendingUp, History as HistoryIcon,
  Play, X, Plus, Minus, Check, Clock, Flame, Award, ChevronRight,
  Search, Trash2, ArrowLeft, ChevronDown, ChevronUp, Settings,
  Scale, CalendarDays, Zap, Target, Activity, Footprints, Bike, PersonStanding
} from "lucide-react";
import {
  ResponsiveContainer, BarChart, Bar, XAxis, YAxis, CartesianGrid,
  Tooltip, LineChart, Line
} from "recharts";

/* ------------------------------------------------------------------ */
/* Design tokens                                                       */
/* ------------------------------------------------------------------ */
const C = {
  bg: "#121319",
  surface: "#1B1D25",
  surface2: "#23252F",
  border: "#2E3039",
  accent: "#C8FF3D",
  accent2: "#FF5A3C",
  text: "#F4F5F0",
  muted: "#8A8D98",
  danger: "#FF5A3C",
};

const DISPLAY_FONT = "'Bebas Neue', sans-serif";
const BODY_FONT = "'Manrope', sans-serif";

/* ------------------------------------------------------------------ */
/* Static data                                                          */
/* ------------------------------------------------------------------ */
const MUSCLES = ["Peito", "Costas", "Pernas", "Ombro", "Braço", "Core", "Cardio"];

const EXERCISES = [
  { id: "e1", name: "Supino Reto", muscle: "Peito", equip: "Barra" },
  { id: "e2", name: "Supino Inclinado", muscle: "Peito", equip: "Barra" },
  { id: "e3", name: "Crucifixo", muscle: "Peito", equip: "Halteres" },
  { id: "e4", name: "Flexão de Braço", muscle: "Peito", equip: "Peso Corporal" },
  { id: "e5", name: "Crossover", muscle: "Peito", equip: "Cabo" },
  { id: "e6", name: "Puxada Alta", muscle: "Costas", equip: "Cabo" },
  { id: "e7", name: "Remada Curvada", muscle: "Costas", equip: "Barra" },
  { id: "e8", name: "Remada Cavalo", muscle: "Costas", equip: "Máquina" },
  { id: "e9", name: "Barra Fixa", muscle: "Costas", equip: "Peso Corporal" },
  { id: "e10", name: "Levantamento Terra", muscle: "Costas", equip: "Barra" },
  { id: "e11", name: "Remada Baixa", muscle: "Costas", equip: "Cabo" },
  { id: "e12", name: "Agachamento Livre", muscle: "Pernas", equip: "Barra" },
  { id: "e13", name: "Leg Press", muscle: "Pernas", equip: "Máquina" },
  { id: "e14", name: "Cadeira Extensora", muscle: "Pernas", equip: "Máquina" },
  { id: "e15", name: "Mesa Flexora", muscle: "Pernas", equip: "Máquina" },
  { id: "e16", name: "Afundo", muscle: "Pernas", equip: "Halteres" },
  { id: "e17", name: "Stiff", muscle: "Pernas", equip: "Barra" },
  { id: "e18", name: "Panturrilha em Pé", muscle: "Pernas", equip: "Máquina" },
  { id: "e19", name: "Cadeira Adutora", muscle: "Pernas", equip: "Máquina" },
  { id: "e20", name: "Desenvolvimento Militar", muscle: "Ombro", equip: "Barra" },
  { id: "e21", name: "Elevação Lateral", muscle: "Ombro", equip: "Halteres" },
  { id: "e22", name: "Elevação Frontal", muscle: "Ombro", equip: "Halteres" },
  { id: "e23", name: "Remada Alta", muscle: "Ombro", equip: "Barra" },
  { id: "e24", name: "Crucifixo Inverso", muscle: "Ombro", equip: "Halteres" },
  { id: "e25", name: "Rosca Direta", muscle: "Braço", equip: "Barra" },
  { id: "e26", name: "Rosca Martelo", muscle: "Braço", equip: "Halteres" },
  { id: "e27", name: "Rosca Scott", muscle: "Braço", equip: "Barra W" },
  { id: "e28", name: "Tríceps Testa", muscle: "Braço", equip: "Barra" },
  { id: "e29", name: "Tríceps Corda", muscle: "Braço", equip: "Cabo" },
  { id: "e30", name: "Tríceps Francês", muscle: "Braço", equip: "Halteres" },
  { id: "e31", name: "Abdominal Supra", muscle: "Core", equip: "Peso Corporal" },
  { id: "e32", name: "Prancha", muscle: "Core", equip: "Peso Corporal" },
  { id: "e33", name: "Elevação de Pernas", muscle: "Core", equip: "Peso Corporal" },
  { id: "e34", name: "Abdominal Oblíquo", muscle: "Core", equip: "Peso Corporal" },
  { id: "e35", name: "Esteira", muscle: "Cardio", equip: "Máquina" },
  { id: "e36", name: "Bicicleta Ergométrica", muscle: "Cardio", equip: "Máquina" },
  { id: "e37", name: "Pular Corda", muscle: "Cardio", equip: "Corda" },
];

const MUSCLE_ICONS = {
  Peito: Dumbbell,
  Costas: Activity,
  Pernas: Footprints,
  Ombro: Dumbbell,
  Braço: Dumbbell,
  Core: Zap,
  Cardio: Bike,
};
const MUSCLE_COLORS = {
  Peito: "#C8FF3D",
  Costas: "#5FD6FF",
  Pernas: "#FFB84D",
  Ombro: "#C8FF3D",
  Braço: "#FF8FD6",
  Core: "#FF5A3C",
  Cardio: "#5FD6FF",
};

function ExerciseBadge({ muscle, size = 38 }) {
  const Icon = MUSCLE_ICONS[muscle] || Dumbbell;
  const color = MUSCLE_COLORS[muscle] || C.accent;
  return (
    <div style={{
      width: size, height: size, borderRadius: size * 0.32, flexShrink: 0,
      background: `${color}1F`, border: `1px solid ${color}55`,
      display: "flex", alignItems: "center", justifyContent: "center",
    }}>
      <Icon size={Math.round(size * 0.46)} color={color} />
    </div>
  );
}

const MUSCLE_TIPS = {
  Peito: "Mantenha os ombros para trás e desça a barra de forma controlada até o peito.",
  Costas: "Puxe com os cotovelos, não com as mãos, e mantenha a coluna neutra.",
  Pernas: "Desça até formar 90° nos joelhos e mantenha o peso nos calcanhares.",
  Ombro: "Evite balançar o corpo; controle a fase excêntrica do movimento.",
  Braço: "Mantenha os cotovelos fixos ao lado do corpo durante todo o movimento.",
  Core: "Contraia o abdômen e respire de forma constante durante a execução.",
  Cardio: "Mantenha um ritmo constante e monitore sua frequência cardíaca.",
};

const TEMPLATES = [
  { id: "t1", name: "Push · Peito, Ombro e Tríceps", tag: "Empurrar", exercises: [
    { exId: "e1", sets: 4, reps: 10 }, { exId: "e2", sets: 3, reps: 10 },
    { exId: "e20", sets: 3, reps: 10 }, { exId: "e21", sets: 3, reps: 12 },
    { exId: "e29", sets: 3, reps: 12 } ] },
  { id: "t2", name: "Pull · Costas e Bíceps", tag: "Puxar", exercises: [
    { exId: "e6", sets: 4, reps: 10 }, { exId: "e7", sets: 3, reps: 10 },
    { exId: "e9", sets: 3, reps: 8 }, { exId: "e25", sets: 3, reps: 12 },
    { exId: "e26", sets: 3, reps: 12 } ] },
  { id: "t3", name: "Legs · Pernas Completo", tag: "Pernas", exercises: [
    { exId: "e12", sets: 4, reps: 8 }, { exId: "e13", sets: 3, reps: 12 },
    { exId: "e14", sets: 3, reps: 12 }, { exId: "e15", sets: 3, reps: 12 },
    { exId: "e18", sets: 4, reps: 15 } ] },
  { id: "t4", name: "Full Body", tag: "Corpo Todo", exercises: [
    { exId: "e12", sets: 3, reps: 10 }, { exId: "e1", sets: 3, reps: 10 },
    { exId: "e7", sets: 3, reps: 10 }, { exId: "e20", sets: 3, reps: 10 },
    { exId: "e32", sets: 3, reps: 30 } ] },
  { id: "t5", name: "Core & Cardio", tag: "Condicionamento", exercises: [
    { exId: "e32", sets: 3, reps: 30 }, { exId: "e31", sets: 3, reps: 15 },
    { exId: "e33", sets: 3, reps: 12 }, { exId: "e37", sets: 3, reps: 60 } ] },
];

/* ------------------------------------------------------------------ */
/* Helpers                                                              */
/* ------------------------------------------------------------------ */
const uid = () => Math.random().toString(36).slice(2, 9);

function fmtDuration(ms) {
  const s = Math.floor(ms / 1000);
  const m = Math.floor(s / 60);
  const h = Math.floor(m / 60);
  if (h > 0) return `${h}h ${m % 60}min`;
  return `${m}min ${s % 60}s`;
}
function fmtClock(sec) {
  const m = Math.floor(sec / 60).toString().padStart(2, "0");
  const s = Math.floor(sec % 60).toString().padStart(2, "0");
  return `${m}:${s}`;
}
function fmtShortDate(d) {
  const date = new Date(d);
  return date.toLocaleDateString("pt-BR", { day: "2-digit", month: "2-digit" });
}
function fmtFullDate(d) {
  const date = new Date(d);
  return date.toLocaleDateString("pt-BR", { weekday: "long", day: "2-digit", month: "long" });
}
function todayKey() {
  return new Date().toISOString().slice(0, 10);
}
function weekKey(dateStr) {
  const d = new Date(dateStr);
  const day = d.getDay() || 7;
  d.setDate(d.getDate() - day + 1);
  return d.toISOString().slice(0, 10);
}
function computeStreak(history) {
  if (!history.length) return 0;
  const days = new Set(history.map((w) => w.date.slice(0, 10)));
  let cursor = new Date();
  if (!days.has(cursor.toISOString().slice(0, 10))) cursor.setDate(cursor.getDate() - 1);
  let streak = 0;
  while (days.has(cursor.toISOString().slice(0, 10))) {
    streak++;
    cursor.setDate(cursor.getDate() - 1);
  }
  return streak;
}

/* ------------------------------------------------------------------ */
/* Storage helpers                                                      */
/* ------------------------------------------------------------------ */
async function safeGet(key, fallback) {
  try {
    const res = await window.storage.get(key, false);
    return res ? JSON.parse(res.value) : fallback;
  } catch (e) {
    return fallback;
  }
}
async function safeSet(key, value) {
  try {
    await window.storage.set(key, JSON.stringify(value), false);
  } catch (e) {
    /* ignore */
  }
}

/* ------------------------------------------------------------------ */
/* Small UI atoms                                                       */
/* ------------------------------------------------------------------ */
function Chip({ active, onClick, children }) {
  return (
    <button
      onClick={onClick}
      style={{
        fontFamily: BODY_FONT,
        background: active ? C.accent : C.surface2,
        color: active ? "#111" : C.muted,
        border: `1px solid ${active ? C.accent : C.border}`,
        borderRadius: 999,
        padding: "6px 14px",
        fontSize: 13,
        fontWeight: 700,
        whiteSpace: "nowrap",
      }}
    >
      {children}
    </button>
  );
}

function PlateBar({ done, total }) {
  const plates = Array.from({ length: total });
  return (
    <div style={{ display: "flex", gap: 4 }}>
      {plates.map((_, i) => (
        <div
          key={i}
          style={{
            height: 8,
            flex: 1,
            borderRadius: 3,
            background: i < done ? C.accent : C.surface2,
            border: `1px solid ${i < done ? C.accent : C.border}`,
            transition: "background .2s",
          }}
        />
      ))}
    </div>
  );
}

function IconBtn({ onClick, children, danger }) {
  return (
    <button
      onClick={onClick}
      style={{
        width: 30, height: 30, borderRadius: 8, display: "flex",
        alignItems: "center", justifyContent: "center",
        background: C.surface2, border: `1px solid ${C.border}`,
        color: danger ? C.danger : C.text,
      }}
    >
      {children}
    </button>
  );
}

function Toast({ message }) {
  if (!message) return null;
  return (
    <div style={{
      position: "absolute", top: 12, left: 12, right: 12, zIndex: 60,
      background: C.accent, color: "#111", fontWeight: 700, fontSize: 13,
      padding: "10px 14px", borderRadius: 10, textAlign: "center",
      fontFamily: BODY_FONT, boxShadow: "0 6px 20px rgba(0,0,0,.4)",
    }}>
      {message}
    </div>
  );
}

function ConfirmModal({ open, title, body, confirmLabel, onConfirm, onCancel }) {
  if (!open) return null;
  return (
    <div style={{
      position: "absolute", inset: 0, background: "rgba(0,0,0,.6)", zIndex: 70,
      display: "flex", alignItems: "flex-end",
    }}>
      <div style={{
        width: "100%", background: C.surface, borderTop: `1px solid ${C.border}`,
        borderRadius: "20px 20px 0 0", padding: 20, fontFamily: BODY_FONT,
      }}>
        <div style={{ fontFamily: DISPLAY_FONT, fontSize: 24, letterSpacing: 0.5 }}>{title}</div>
        <div style={{ color: C.muted, fontSize: 14, marginTop: 6, marginBottom: 18 }}>{body}</div>
        <div style={{ display: "flex", gap: 10 }}>
          <button onClick={onCancel} style={{
            flex: 1, padding: "12px 0", borderRadius: 10, background: C.surface2,
            border: `1px solid ${C.border}`, color: C.text, fontWeight: 700,
          }}>Cancelar</button>
          <button onClick={onConfirm} style={{
            flex: 1, padding: "12px 0", borderRadius: 10, background: C.danger,
            border: "none", color: "#111", fontWeight: 700,
          }}>{confirmLabel}</button>
        </div>
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Rest timer overlay (signature element: circular plate dial)          */
/* ------------------------------------------------------------------ */
function RestTimer({ timer, onAdjust, onSkip }) {
  if (!timer) return null;
  const r = 30;
  const circ = 2 * Math.PI * r;
  const progress = timer.total > 0 ? timer.seconds / timer.total : 0;
  const offset = circ * (1 - progress);
  return (
    <div style={{
      position: "absolute", left: 12, right: 12, bottom: 78, zIndex: 50,
      background: C.surface, border: `1px solid ${C.border}`, borderRadius: 16,
      padding: "10px 14px", display: "flex", alignItems: "center", gap: 14,
      boxShadow: "0 8px 24px rgba(0,0,0,.45)",
    }}>
      <svg width="68" height="68" style={{ flexShrink: 0 }}>
        <circle cx="34" cy="34" r={r} stroke={C.border} strokeWidth="6" fill="none" />
        <circle
          cx="34" cy="34" r={r} stroke={C.accent} strokeWidth="6" fill="none"
          strokeDasharray={circ} strokeDashoffset={offset} strokeLinecap="round"
          transform="rotate(-90 34 34)" style={{ transition: "stroke-dashoffset 1s linear" }}
        />
        <text x="34" y="39" textAnchor="middle" fill={C.text} fontSize="13"
          fontFamily={DISPLAY_FONT}>{fmtClock(timer.seconds)}</text>
      </svg>
      <div style={{ flex: 1, fontFamily: BODY_FONT }}>
        <div style={{ fontSize: 12, color: C.muted, fontWeight: 700, textTransform: "uppercase" }}>Descanso</div>
        <div style={{ display: "flex", gap: 6, marginTop: 6 }}>
          <button onClick={() => onAdjust(-15)} style={{
            background: C.surface2, border: `1px solid ${C.border}`, color: C.text,
            borderRadius: 8, width: 30, height: 26, fontWeight: 700,
          }}>-15</button>
          <button onClick={() => onAdjust(15)} style={{
            background: C.surface2, border: `1px solid ${C.border}`, color: C.text,
            borderRadius: 8, width: 30, height: 26, fontWeight: 700,
          }}>+15</button>
        </div>
      </div>
      <button onClick={onSkip} style={{
        background: C.accent, color: "#111", border: "none", borderRadius: 10,
        padding: "8px 12px", fontWeight: 800, fontSize: 12,
      }}>PULAR</button>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Main App                                                              */
/* ------------------------------------------------------------------ */
export default function App() {
  const [tab, setTab] = useState("home");
  const [loaded, setLoaded] = useState(false);
  const [history, setHistory] = useState([]);
  const [bodyLog, setBodyLog] = useState([]);
  const [settings, setSettings] = useState({ unit: "kg" });
  const [session, setSession] = useState(null);
  const [restTimer, setRestTimer] = useState(null);
  const [toast, setToast] = useState(null);
  const [summary, setSummary] = useState(null);
  const [confirm, setConfirm] = useState(null);
  const [showAddExercise, setShowAddExercise] = useState(false);
  const [showSettings, setShowSettings] = useState(false);
  const [libSearch, setLibSearch] = useState("");
  const [libFilter, setLibFilter] = useState("Todos");
  const [expandedEx, setExpandedEx] = useState(null);
  const [expandedHistory, setExpandedHistory] = useState(null);
  const [bwInput, setBwInput] = useState("");

  useEffect(() => {
    (async () => {
      const [h, b, s] = await Promise.all([
        safeGet("wt-history", []),
        safeGet("wt-bodylog", []),
        safeGet("wt-settings", { unit: "kg" }),
      ]);
      setHistory(h);
      setBodyLog(b);
      setSettings(s);
      setLoaded(true);
    })();
  }, []);

  // rest timer countdown
  useEffect(() => {
    if (!restTimer || restTimer.seconds <= 0) return;
    const id = setInterval(() => {
      setRestTimer((t) => {
        if (!t) return t;
        if (t.seconds <= 1) return null;
        return { ...t, seconds: t.seconds - 1 };
      });
    }, 1000);
    return () => clearInterval(id);
  }, [restTimer]);

  // elapsed workout time ticker
  const [, forceTick] = useState(0);
  useEffect(() => {
    if (!session) return;
    const id = setInterval(() => forceTick((n) => n + 1), 1000);
    return () => clearInterval(id);
  }, [session]);

  useEffect(() => {
    if (!toast) return;
    const id = setTimeout(() => setToast(null), 2200);
    return () => clearTimeout(id);
  }, [toast]);

  const saveHistory = (h) => { setHistory(h); safeSet("wt-history", h); };
  const saveBodyLog = (b) => { setBodyLog(b); safeSet("wt-bodylog", b); };
  const saveSettings = (s) => { setSettings(s); safeSet("wt-settings", s); };

  const exercisePRs = useMemo(() => {
    const map = {};
    history.forEach((w) =>
      w.exercises.forEach((ex) =>
        ex.sets.forEach((s) => {
          if (s.weight > 0 && (!map[ex.exId] || s.weight > map[ex.exId].weight)) {
            map[ex.exId] = { weight: s.weight, reps: s.reps, date: w.date, name: ex.name };
          }
        })
      )
    );
    return map;
  }, [history]);

  function getLastPerformance(exId) {
    for (let i = history.length - 1; i >= 0; i--) {
      const ex = history[i].exercises.find((e) => e.exId === exId);
      if (ex && ex.sets.length) return ex.sets[ex.sets.length - 1];
    }
    return null;
  }

  const streak = useMemo(() => computeStreak(history), [history]);

  const weekStats = useMemo(() => {
    const wk = weekKey(todayKey());
    const items = history.filter((w) => weekKey(w.date) === wk);
    return {
      count: items.length,
      volume: Math.round(items.reduce((a, w) => a + w.totalVolume, 0)),
      time: items.reduce((a, w) => a + w.duration, 0),
    };
  }, [history]);

  const weeklyVolumeData = useMemo(() => {
    const map = {};
    history.forEach((w) => {
      const k = weekKey(w.date);
      map[k] = (map[k] || 0) + w.totalVolume;
    });
    const keys = Object.keys(map).sort().slice(-8);
    return keys.map((k) => ({ label: fmtShortDate(k), volume: Math.round(map[k]) }));
  }, [history]);

  const bodyWeightData = useMemo(() => {
    return [...bodyLog].sort((a, b) => new Date(a.date) - new Date(b.date))
      .map((e) => ({ label: fmtShortDate(e.date), peso: e.weight }));
  }, [bodyLog]);

  /* -------------------- workout session actions -------------------- */
  function startTemplate(template) {
    const exs = template.exercises.map((te) => {
      const ex = EXERCISES.find((e) => e.id === te.exId);
      const last = getLastPerformance(te.exId);
      const sets = Array.from({ length: te.sets }, () => ({
        weight: last ? last.weight : 0,
        reps: last ? last.reps : te.reps,
        done: false,
      }));
      return { exId: te.exId, name: ex.name, muscle: ex.muscle, sets };
    });
    setSession({ name: template.name, startTime: Date.now(), exercises: exs });
    setTab("train");
  }

  function startFreeWorkout() {
    setSession({ name: "Treino Livre", startTime: Date.now(), exercises: [] });
    setTab("train");
    setShowAddExercise(true);
  }

  function addExerciseToSession(exId) {
    const ex = EXERCISES.find((e) => e.id === exId);
    const last = getLastPerformance(exId);
    setSession((s) => ({
      ...s,
      exercises: [...s.exercises, {
        exId, name: ex.name, muscle: ex.muscle,
        sets: [{ weight: last ? last.weight : 0, reps: last ? last.reps : 10, done: false }],
      }],
    }));
    setShowAddExercise(false);
    setToast(`${ex.name} adicionado`);
  }

  function updateSet(exIdx, setIdx, field, value) {
    setSession((s) => {
      const exercises = [...s.exercises];
      const sets = [...exercises[exIdx].sets];
      sets[setIdx] = { ...sets[setIdx], [field]: value };
      exercises[exIdx] = { ...exercises[exIdx], sets };
      return { ...s, exercises };
    });
  }

  function toggleSetDone(exIdx, setIdx) {
    setSession((s) => {
      const exercises = [...s.exercises];
      const sets = [...exercises[exIdx].sets];
      const nowDone = !sets[setIdx].done;
      sets[setIdx] = { ...sets[setIdx], done: nowDone };
      exercises[exIdx] = { ...exercises[exIdx], sets };
      if (nowDone) setRestTimer({ seconds: 90, total: 90 });
      return { ...s, exercises };
    });
  }

  function addSet(exIdx) {
    setSession((s) => {
      const exercises = [...s.exercises];
      const sets = [...exercises[exIdx].sets];
      const last = sets[sets.length - 1];
      sets.push({ weight: last ? last.weight : 0, reps: last ? last.reps : 10, done: false });
      exercises[exIdx] = { ...exercises[exIdx], sets };
      return { ...s, exercises };
    });
  }

  function removeSet(exIdx, setIdx) {
    setSession((s) => {
      const exercises = [...s.exercises];
      const sets = exercises[exIdx].sets.filter((_, i) => i !== setIdx);
      if (!sets.length) return s;
      exercises[exIdx] = { ...exercises[exIdx], sets };
      return { ...s, exercises };
    });
  }

  function removeExercise(exIdx) {
    setSession((s) => ({ ...s, exercises: s.exercises.filter((_, i) => i !== exIdx) }));
  }

  function finishWorkout() {
    const doneExercises = session.exercises
      .map((e) => ({ exId: e.exId, name: e.name, sets: e.sets.filter((s) => s.done) }))
      .filter((e) => e.sets.length > 0);
    if (!doneExercises.length) {
      setToast("Marque ao menos uma série concluída");
      return;
    }
    const totalVolume = doneExercises.reduce(
      (a, e) => a + e.sets.reduce((sa, s) => sa + s.weight * s.reps, 0), 0
    );
    const duration = Date.now() - session.startTime;
    const prs = [];
    doneExercises.forEach((e) => {
      const maxSet = e.sets.reduce((m, s) => (s.weight > m.weight ? s : m), { weight: 0, reps: 0 });
      const prevBest = exercisePRs[e.exId];
      if (maxSet.weight > 0 && (!prevBest || maxSet.weight > prevBest.weight)) {
        prs.push({ name: e.name, weight: maxSet.weight, reps: maxSet.reps });
      }
    });
    const newWorkout = {
      id: uid(), date: new Date().toISOString(), name: session.name,
      exercises: doneExercises, totalVolume, duration,
    };
    saveHistory([...history, newWorkout]);
    setSummary({ ...newWorkout, prs });
    setSession(null);
    setRestTimer(null);
  }

  function cancelWorkout() {
    setConfirm({
      title: "Cancelar treino?",
      body: "Todo o progresso desta sessão será perdido.",
      confirmLabel: "Cancelar treino",
      onConfirm: () => { setSession(null); setRestTimer(null); setConfirm(null); },
    });
  }

  function deleteWorkout(id) {
    setConfirm({
      title: "Excluir treino?",
      body: "Essa ação não pode ser desfeita.",
      confirmLabel: "Excluir",
      onConfirm: () => { saveHistory(history.filter((w) => w.id !== id)); setConfirm(null); },
    });
  }

  function resetAllData() {
    setConfirm({
      title: "Apagar todos os dados?",
      body: "Histórico, recordes e peso corporal serão apagados permanentemente.",
      confirmLabel: "Apagar tudo",
      onConfirm: () => {
        saveHistory([]); saveBodyLog([]);
        setConfirm(null); setShowSettings(false);
        setToast("Dados apagados");
      },
    });
  }

  function addBodyWeight() {
    const val = parseFloat(bwInput.replace(",", "."));
    if (!val || val <= 0) { setToast("Informe um peso válido"); return; }
    saveBodyLog([...bodyLog, { date: new Date().toISOString(), weight: val }]);
    setBwInput("");
    setToast("Peso registrado");
  }

  if (!loaded) {
    return (
      <div style={{ background: C.bg, height: "100vh", display: "flex", alignItems: "center", justifyContent: "center" }}>
        <div style={{ color: C.accent, fontFamily: DISPLAY_FONT, fontSize: 28, letterSpacing: 1 }}>CARREGANDO…</div>
      </div>
    );
  }

  return (
    <div style={{
      fontFamily: BODY_FONT, background: C.bg, color: C.text,
      maxWidth: 430, margin: "0 auto", height: "100vh", display: "flex",
      flexDirection: "column", position: "relative", overflow: "hidden",
      border: `1px solid ${C.border}`,
    }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Manrope:wght@400;500;600;700;800&display=swap');
        * { box-sizing: border-box; }
        input, button { font-family: ${BODY_FONT}; }
        input:focus, button:focus { outline: 2px solid ${C.accent}; outline-offset: 1px; }
        ::-webkit-scrollbar { width: 0; height: 0; }
      `}</style>

      <Toast message={toast} />
      <ConfirmModal
        open={!!confirm}
        title={confirm?.title} body={confirm?.body} confirmLabel={confirm?.confirmLabel}
        onConfirm={confirm?.onConfirm} onCancel={() => setConfirm(null)}
      />

      {summary && (
        <SummaryOverlay summary={summary} onClose={() => { setSummary(null); setTab("home"); }} />
      )}
      {showSettings && (
        <SettingsOverlay
          settings={settings} onChangeUnit={(u) => saveSettings({ ...settings, unit: u })}
          onReset={resetAllData} onClose={() => setShowSettings(false)}
        />
      )}
      {showAddExercise && (
        <AddExerciseOverlay onPick={addExerciseToSession} onClose={() => setShowAddExercise(false)} />
      )}

      <div style={{ flex: 1, overflowY: "auto", padding: "20px 16px 12px" }}>
        {tab === "home" && (
          <HomeScreen
            streak={streak} weekStats={weekStats} history={history}
            onStartTemplate={startTemplate} onStartFree={startFreeWorkout}
            onOpenSettings={() => setShowSettings(true)}
            onGoHistory={() => setTab("history")}
          />
        )}
        {tab === "train" && (
          session ? (
            <ActiveWorkout
              session={session} elapsedTick={forceTick}
              onToggleSet={toggleSetDone} onUpdateSet={updateSet}
              onAddSet={addSet} onRemoveSet={removeSet} onRemoveExercise={removeExercise}
              onAddExercise={() => setShowAddExercise(true)}
              onFinish={finishWorkout} onCancel={cancelWorkout}
            />
          ) : (
            <TrainScreen onStartTemplate={startTemplate} onStartFree={startFreeWorkout} />
          )
        )}
        {tab === "library" && (
          <LibraryScreen
            search={libSearch} setSearch={setLibSearch}
            filter={libFilter} setFilter={setLibFilter}
            expanded={expandedEx} setExpanded={setExpandedEx}
            prMap={exercisePRs} unit={settings.unit}
          />
        )}
        {tab === "progress" && (
          <ProgressScreen
            weeklyVolumeData={weeklyVolumeData} bodyWeightData={bodyWeightData}
            prMap={exercisePRs} bwInput={bwInput} setBwInput={setBwInput}
            onAddBodyWeight={addBodyWeight} unit={settings.unit}
          />
        )}
        {tab === "history" && (
          <HistoryScreen
            history={history} expanded={expandedHistory} setExpanded={setExpandedHistory}
            onDelete={deleteWorkout} unit={settings.unit}
          />
        )}
      </div>

      {restTimer && session && (
        <RestTimer
          timer={restTimer}
          onAdjust={(d) => setRestTimer((t) => t ? { ...t, seconds: Math.max(1, t.seconds + d), total: Math.max(t.total, t.seconds + d) } : t)}
          onSkip={() => setRestTimer(null)}
        />
      )}

      <BottomNav tab={tab} setTab={setTab} hasSession={!!session} />
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Bottom navigation                                                    */
/* ------------------------------------------------------------------ */
function BottomNav({ tab, setTab, hasSession }) {
  const items = [
    { id: "home", label: "Início", icon: Home },
    { id: "train", label: "Treinar", icon: Dumbbell },
    { id: "library", label: "Biblioteca", icon: BookOpen },
    { id: "progress", label: "Progresso", icon: TrendingUp },
    { id: "history", label: "Histórico", icon: HistoryIcon },
  ];
  return (
    <div style={{
      display: "flex", borderTop: `1px solid ${C.border}`, background: C.surface,
      padding: "8px 4px 12px",
    }}>
      {items.map((it) => {
        const Icon = it.icon;
        const active = tab === it.id;
        return (
          <button key={it.id} onClick={() => setTab(it.id)} style={{
            flex: 1, display: "flex", flexDirection: "column", alignItems: "center",
            gap: 3, background: "transparent", border: "none", position: "relative",
            color: active ? C.accent : C.muted, padding: "4px 0",
          }}>
            <Icon size={20} strokeWidth={active ? 2.5 : 2} />
            <span style={{ fontSize: 10, fontWeight: 700 }}>{it.label}</span>
            {it.id === "train" && hasSession && (
              <span style={{
                position: "absolute", top: 0, right: "28%", width: 7, height: 7,
                borderRadius: 99, background: C.accent2,
              }} />
            )}
          </button>
        );
      })}
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Home                                                                  */
/* ------------------------------------------------------------------ */
function HomeScreen({ streak, weekStats, history, onStartTemplate, onStartFree, onOpenSettings, onGoHistory }) {
  const hour = new Date().getHours();
  const greeting = hour < 12 ? "Bom dia" : hour < 18 ? "Boa tarde" : "Boa noite";
  const recent = [...history].slice(-3).reverse();

  return (
    <div>
      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start" }}>
        <div>
          <div style={{ color: C.muted, fontSize: 13, fontWeight: 600 }}>{greeting} 💪</div>
          <div style={{ fontFamily: DISPLAY_FONT, fontSize: 34, letterSpacing: 0.5, lineHeight: 1 }}>
            VAMOS TREINAR
          </div>
        </div>
        <button onClick={onOpenSettings} style={{
          width: 36, height: 36, borderRadius: 10, background: C.surface,
          border: `1px solid ${C.border}`, color: C.muted, display: "flex",
          alignItems: "center", justifyContent: "center",
        }}>
          <Settings size={17} />
        </button>
      </div>

      <div style={{
        marginTop: 18, background: C.surface, border: `1px solid ${C.border}`,
        borderRadius: 16, padding: 16, display: "flex", alignItems: "center", gap: 14,
      }}>
        <div style={{
          width: 54, height: 54, borderRadius: 14, background: "rgba(200,255,61,.12)",
          display: "flex", alignItems: "center", justifyContent: "center", flexShrink: 0,
        }}>
          <Flame color={C.accent} size={26} />
        </div>
        <div>
          <div style={{ fontFamily: DISPLAY_FONT, fontSize: 28, lineHeight: 1 }}>{streak} DIAS</div>
          <div style={{ color: C.muted, fontSize: 12, fontWeight: 600 }}>de sequência de treino</div>
        </div>
      </div>

      <div style={{ display: "flex", gap: 10, marginTop: 12 }}>
        {[
          { label: "Treinos", value: weekStats.count },
          { label: "Volume (kg)", value: weekStats.volume.toLocaleString("pt-BR") },
          { label: "Tempo", value: weekStats.time ? fmtDuration(weekStats.time) : "0min" },
        ].map((s) => (
          <div key={s.label} style={{
            flex: 1, background: C.surface, border: `1px solid ${C.border}`,
            borderRadius: 14, padding: "10px 8px", textAlign: "center",
          }}>
            <div style={{ fontFamily: DISPLAY_FONT, fontSize: 20 }}>{s.value}</div>
            <div style={{ color: C.muted, fontSize: 10.5, fontWeight: 700, textTransform: "uppercase" }}>{s.label}</div>
          </div>
        ))}
      </div>

      <div style={{ marginTop: 22, fontFamily: DISPLAY_FONT, fontSize: 20, letterSpacing: 0.5 }}>INÍCIO RÁPIDO</div>
      <div style={{ display: "flex", overflowX: "auto", gap: 10, marginTop: 10, paddingBottom: 4 }}>
        {TEMPLATES.map((t) => (
          <button key={t.id} onClick={() => onStartTemplate(t)} style={{
            minWidth: 150, background: C.surface, border: `1px solid ${C.border}`,
            borderRadius: 14, padding: 14, textAlign: "left", flexShrink: 0,
          }}>
            <div style={{ color: C.accent, fontSize: 11, fontWeight: 800, textTransform: "uppercase" }}>{t.tag}</div>
            <div style={{ fontWeight: 700, fontSize: 14, marginTop: 6, lineHeight: 1.25 }}>{t.name}</div>
            <div style={{ color: C.muted, fontSize: 12, marginTop: 8 }}>{t.exercises.length} exercícios</div>
          </button>
        ))}
        <button onClick={onStartFree} style={{
          minWidth: 130, background: "rgba(200,255,61,.1)", border: `1px dashed ${C.accent}`,
          borderRadius: 14, padding: 14, textAlign: "left", flexShrink: 0, color: C.accent,
        }}>
          <Plus size={20} />
          <div style={{ fontWeight: 800, fontSize: 13, marginTop: 8 }}>Treino Livre</div>
        </button>
      </div>

      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginTop: 22 }}>
        <div style={{ fontFamily: DISPLAY_FONT, fontSize: 20, letterSpacing: 0.5 }}>ATIVIDADE RECENTE</div>
        {history.length > 0 && (
          <button onClick={onGoHistory} style={{ background: "none", border: "none", color: C.muted, fontSize: 12, fontWeight: 700, display: "flex", alignItems: "center", gap: 2 }}>
            Ver tudo <ChevronRight size={14} />
          </button>
        )}
      </div>
      {recent.length === 0 ? (
        <div style={{ color: C.muted, fontSize: 13, marginTop: 10, lineHeight: 1.5 }}>
          Nenhum treino ainda. Escolha um treino acima para começar sua jornada.
        </div>
      ) : (
        <div style={{ display: "flex", flexDirection: "column", gap: 8, marginTop: 10 }}>
          {recent.map((w) => (
            <div key={w.id} style={{
              background: C.surface, border: `1px solid ${C.border}`, borderRadius: 12,
              padding: 12, display: "flex", justifyContent: "space-between", alignItems: "center",
            }}>
              <div>
                <div style={{ fontWeight: 700, fontSize: 14 }}>{w.name}</div>
                <div style={{ color: C.muted, fontSize: 12, marginTop: 2 }}>{fmtFullDate(w.date)}</div>
              </div>
              <div style={{ textAlign: "right" }}>
                <div style={{ fontFamily: DISPLAY_FONT, fontSize: 18, color: C.accent }}>{Math.round(w.totalVolume)}</div>
                <div style={{ color: C.muted, fontSize: 10 }}>kg volume</div>
              </div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Train (selection) screen                                             */
/* ------------------------------------------------------------------ */
function TrainScreen({ onStartTemplate, onStartFree }) {
  return (
    <div>
      <div style={{ fontFamily: DISPLAY_FONT, fontSize: 30, letterSpacing: 0.5 }}>ESCOLHA SEU TREINO</div>
      <div style={{ color: C.muted, fontSize: 13, marginTop: 4 }}>Selecione um modelo pronto ou monte o seu.</div>

      <div style={{ display: "flex", flexDirection: "column", gap: 10, marginTop: 18 }}>
        {TEMPLATES.map((t) => (
          <button key={t.id} onClick={() => onStartTemplate(t)} style={{
            background: C.surface, border: `1px solid ${C.border}`, borderRadius: 16,
            padding: 16, textAlign: "left", display: "flex", justifyContent: "space-between", alignItems: "center",
          }}>
            <div>
              <div style={{ color: C.accent, fontSize: 11, fontWeight: 800, textTransform: "uppercase" }}>{t.tag}</div>
              <div style={{ fontWeight: 800, fontSize: 16, marginTop: 4 }}>{t.name}</div>
              <div style={{ color: C.muted, fontSize: 12, marginTop: 4 }}>
                {t.exercises.map((e) => EXERCISES.find((x) => x.id === e.exId).name).join(" · ")}
              </div>
            </div>
            <div style={{
              width: 38, height: 38, borderRadius: 10, background: C.accent,
              display: "flex", alignItems: "center", justifyContent: "center", flexShrink: 0, marginLeft: 10,
            }}>
              <Play size={16} color="#111" fill="#111" />
            </div>
          </button>
        ))}

        <button onClick={onStartFree} style={{
          background: "rgba(200,255,61,.08)", border: `1px dashed ${C.accent}`, borderRadius: 16,
          padding: 16, textAlign: "left", display: "flex", alignItems: "center", gap: 12,
        }}>
          <div style={{
            width: 38, height: 38, borderRadius: 10, background: "rgba(200,255,61,.15)",
            display: "flex", alignItems: "center", justifyContent: "center", flexShrink: 0,
          }}>
            <Plus size={18} color={C.accent} />
          </div>
          <div>
            <div style={{ fontWeight: 800, fontSize: 15, color: C.accent }}>Treino Livre</div>
            <div style={{ color: C.muted, fontSize: 12 }}>Monte sua sessão do zero</div>
          </div>
        </button>
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Active workout                                                        */
/* ------------------------------------------------------------------ */
function ActiveWorkout({ session, onToggleSet, onUpdateSet, onAddSet, onRemoveSet, onRemoveExercise, onAddExercise, onFinish, onCancel }) {
  const totalSets = session.exercises.reduce((a, e) => a + e.sets.length, 0);
  const doneSets = session.exercises.reduce((a, e) => a + e.sets.filter((s) => s.done).length, 0);
  const elapsed = Date.now() - session.startTime;

  return (
    <div>
      <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center" }}>
        <div>
          <div style={{ color: C.muted, fontSize: 12, fontWeight: 700, textTransform: "uppercase" }}>Em andamento</div>
          <div style={{ fontFamily: DISPLAY_FONT, fontSize: 26, letterSpacing: 0.5 }}>{session.name}</div>
        </div>
        <button onClick={onCancel} style={{
          width: 34, height: 34, borderRadius: 10, background: C.surface,
          border: `1px solid ${C.border}`, color: C.danger, display: "flex",
          alignItems: "center", justifyContent: "center",
        }}>
          <X size={17} />
        </button>
      </div>

      <div style={{ display: "flex", gap: 16, marginTop: 12 }}>
        <div style={{ display: "flex", alignItems: "center", gap: 6, color: C.muted, fontSize: 13 }}>
          <Clock size={15} /> {fmtDuration(elapsed)}
        </div>
        <div style={{ display: "flex", alignItems: "center", gap: 6, color: C.muted, fontSize: 13 }}>
          <Target size={15} /> {doneSets}/{totalSets} séries
        </div>
      </div>
      <div style={{ marginTop: 10 }}><PlateBar done={doneSets} total={Math.max(totalSets, 1)} /></div>

      <div style={{ display: "flex", flexDirection: "column", gap: 14, marginTop: 18 }}>
        {session.exercises.map((ex, exIdx) => (
          <div key={exIdx} style={{ background: C.surface, border: `1px solid ${C.border}`, borderRadius: 16, padding: 14 }}>
            <div style={{ display: "flex", justifyContent: "space-between", alignItems: "flex-start", gap: 10 }}>
              <div style={{ display: "flex", alignItems: "center", gap: 10, flex: 1 }}>
                <ExerciseBadge muscle={ex.muscle} size={34} />
                <div>
                  <div style={{ fontWeight: 800, fontSize: 15 }}>{ex.name}</div>
                  <div style={{ color: C.muted, fontSize: 11, fontWeight: 700, textTransform: "uppercase" }}>{ex.muscle}</div>
                </div>
              </div>
              <IconBtn danger onClick={() => onRemoveExercise(exIdx)}><Trash2 size={14} /></IconBtn>
            </div>

            <div style={{ display: "flex", color: C.muted, fontSize: 11, fontWeight: 700, marginTop: 12, paddingLeft: 4 }}>
              <div style={{ width: 26 }}>#</div>
              <div style={{ flex: 1 }}>PESO (KG)</div>
              <div style={{ flex: 1 }}>REPS</div>
              <div style={{ width: 34 }}></div>
              <div style={{ width: 30 }}></div>
            </div>
            {ex.sets.map((s, setIdx) => (
              <div key={setIdx} style={{ display: "flex", alignItems: "center", marginTop: 6, gap: 6 }}>
                <div style={{ width: 26, color: C.muted, fontSize: 13, fontWeight: 700 }}>{setIdx + 1}</div>
                <input type="number" inputMode="decimal" value={s.weight}
                  onChange={(e) => onUpdateSet(exIdx, setIdx, "weight", parseFloat(e.target.value) || 0)}
                  style={{
                    flex: 1, background: C.surface2, border: `1px solid ${C.border}`, borderRadius: 8,
                    color: C.text, padding: "8px 6px", fontSize: 14, fontWeight: 700, width: 0,
                  }} />
                <input type="number" inputMode="numeric" value={s.reps}
                  onChange={(e) => onUpdateSet(exIdx, setIdx, "reps", parseInt(e.target.value) || 0)}
                  style={{
                    flex: 1, background: C.surface2, border: `1px solid ${C.border}`, borderRadius: 8,
                    color: C.text, padding: "8px 6px", fontSize: 14, fontWeight: 700, width: 0,
                  }} />
                <button onClick={() => onToggleSet(exIdx, setIdx)} style={{
                  width: 34, height: 34, borderRadius: 8, flexShrink: 0,
                  background: s.done ? C.accent : C.surface2,
                  border: `1px solid ${s.done ? C.accent : C.border}`,
                  display: "flex", alignItems: "center", justifyContent: "center",
                }}>
                  <Check size={16} color={s.done ? "#111" : C.muted} />
                </button>
                {ex.sets.length > 1 && (
                  <button onClick={() => onRemoveSet(exIdx, setIdx)} style={{
                    width: 30, height: 34, background: "none", border: "none", color: C.muted, flexShrink: 0,
                  }}>
                    <X size={14} />
                  </button>
                )}
              </div>
            ))}
            <button onClick={() => onAddSet(exIdx)} style={{
              marginTop: 10, width: "100%", padding: "8px 0", borderRadius: 8,
              background: C.surface2, border: `1px dashed ${C.border}`, color: C.muted,
              fontSize: 12, fontWeight: 700, display: "flex", alignItems: "center", justifyContent: "center", gap: 5,
            }}>
              <Plus size={13} /> ADICIONAR SÉRIE
            </button>
          </div>
        ))}
      </div>

      <button onClick={onAddExercise} style={{
        marginTop: 14, width: "100%", padding: "13px 0", borderRadius: 12,
        background: C.surface, border: `1px dashed ${C.accent}`, color: C.accent,
        fontSize: 13, fontWeight: 800, display: "flex", alignItems: "center", justifyContent: "center", gap: 6,
      }}>
        <Plus size={15} /> ADICIONAR EXERCÍCIO
      </button>

      <button onClick={onFinish} style={{
        marginTop: 12, width: "100%", padding: "16px 0", borderRadius: 14,
        background: C.accent, border: "none", color: "#111",
        fontSize: 15, fontWeight: 800, letterSpacing: 0.5,
      }}>
        FINALIZAR TREINO
      </button>
      <div style={{ height: 90 }} />
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Add exercise overlay                                                  */
/* ------------------------------------------------------------------ */
function AddExerciseOverlay({ onPick, onClose }) {
  const [q, setQ] = useState("");
  const [filter, setFilter] = useState("Todos");
  const list = EXERCISES.filter((e) =>
    (filter === "Todos" || e.muscle === filter) &&
    e.name.toLowerCase().includes(q.toLowerCase())
  );
  return (
    <div style={{ position: "absolute", inset: 0, background: C.bg, zIndex: 65, display: "flex", flexDirection: "column" }}>
      <div style={{ padding: "18px 16px 10px", display: "flex", alignItems: "center", gap: 10 }}>
        <IconBtn onClick={onClose}><ArrowLeft size={16} /></IconBtn>
        <div style={{ fontFamily: DISPLAY_FONT, fontSize: 22 }}>ADICIONAR EXERCÍCIO</div>
      </div>
      <div style={{ padding: "0 16px" }}>
        <div style={{ position: "relative" }}>
          <Search size={15} color={C.muted} style={{ position: "absolute", left: 12, top: 12 }} />
          <input value={q} onChange={(e) => setQ(e.target.value)} placeholder="Buscar exercício..."
            style={{
              width: "100%", background: C.surface, border: `1px solid ${C.border}`, borderRadius: 10,
              padding: "10px 12px 10px 34px", color: C.text, fontSize: 14,
            }} />
        </div>
        <div style={{ display: "flex", gap: 8, overflowX: "auto", marginTop: 10, paddingBottom: 4 }}>
          <Chip active={filter === "Todos"} onClick={() => setFilter("Todos")}>Todos</Chip>
          {MUSCLES.map((m) => <Chip key={m} active={filter === m} onClick={() => setFilter(m)}>{m}</Chip>)}
        </div>
      </div>
      <div style={{ flex: 1, overflowY: "auto", padding: "10px 16px 24px" }}>
        {list.map((e) => (
          <button key={e.id} onClick={() => onPick(e.id)} style={{
            width: "100%", display: "flex", justifyContent: "space-between", alignItems: "center",
            background: C.surface, border: `1px solid ${C.border}`, borderRadius: 12,
            padding: "12px 14px", marginBottom: 8, textAlign: "left", gap: 12,
          }}>
            <ExerciseBadge muscle={e.muscle} size={34} />
            <div style={{ flex: 1 }}>
              <div style={{ fontWeight: 700, fontSize: 14 }}>{e.name}</div>
              <div style={{ color: C.muted, fontSize: 11 }}>{e.muscle} · {e.equip}</div>
            </div>
            <Plus size={16} color={C.accent} />
          </button>
        ))}
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Library                                                               */
/* ------------------------------------------------------------------ */
function LibraryScreen({ search, setSearch, filter, setFilter, expanded, setExpanded, prMap, unit }) {
  const list = EXERCISES.filter((e) =>
    (filter === "Todos" || e.muscle === filter) &&
    e.name.toLowerCase().includes(search.toLowerCase())
  );
  return (
    <div>
      <div style={{ fontFamily: DISPLAY_FONT, fontSize: 30, letterSpacing: 0.5 }}>BIBLIOTECA</div>
      <div style={{ color: C.muted, fontSize: 13, marginTop: 4 }}>{EXERCISES.length} exercícios disponíveis</div>

      <div style={{ position: "relative", marginTop: 14 }}>
        <Search size={15} color={C.muted} style={{ position: "absolute", left: 12, top: 12 }} />
        <input value={search} onChange={(e) => setSearch(e.target.value)} placeholder="Buscar exercício..."
          style={{
            width: "100%", background: C.surface, border: `1px solid ${C.border}`, borderRadius: 10,
            padding: "10px 12px 10px 34px", color: C.text, fontSize: 14,
          }} />
      </div>
      <div style={{ display: "flex", gap: 8, overflowX: "auto", marginTop: 10, paddingBottom: 4 }}>
        <Chip active={filter === "Todos"} onClick={() => setFilter("Todos")}>Todos</Chip>
        {MUSCLES.map((m) => <Chip key={m} active={filter === m} onClick={() => setFilter(m)}>{m}</Chip>)}
      </div>

      <div style={{ display: "flex", flexDirection: "column", gap: 8, marginTop: 14 }}>
        {list.map((e) => {
          const isOpen = expanded === e.id;
          const pr = prMap[e.id];
          return (
            <div key={e.id} style={{ background: C.surface, border: `1px solid ${C.border}`, borderRadius: 12, overflow: "hidden" }}>
              <button onClick={() => setExpanded(isOpen ? null : e.id)} style={{
                width: "100%", display: "flex", justifyContent: "space-between", alignItems: "center",
                padding: "12px 14px", textAlign: "left", background: "none", border: "none", gap: 12,
              }}>
                <ExerciseBadge muscle={e.muscle} />
                <div style={{ flex: 1 }}>
                  <div style={{ fontWeight: 700, fontSize: 14 }}>{e.name}</div>
                  <div style={{ color: C.muted, fontSize: 11 }}>{e.muscle} · {e.equip}</div>
                </div>
                {isOpen ? <ChevronUp size={16} color={C.muted} /> : <ChevronDown size={16} color={C.muted} />}
              </button>
              {isOpen && (
                <div style={{ padding: "0 14px 14px", borderTop: `1px solid ${C.border}` }}>
                  <div style={{ color: C.muted, fontSize: 12.5, marginTop: 10, lineHeight: 1.5 }}>{MUSCLE_TIPS[e.muscle]}</div>
                  {pr ? (
                    <div style={{ display: "flex", alignItems: "center", gap: 8, marginTop: 10 }}>
                      <Award size={15} color={C.accent} />
                      <div style={{ fontSize: 13, fontWeight: 700 }}>
                        Recorde: {pr.weight}{unit} × {pr.reps} reps
                      </div>
                    </div>
                  ) : (
                    <div style={{ fontSize: 12, color: C.muted, marginTop: 10 }}>Ainda sem recorde registrado.</div>
                  )}
                </div>
              )}
            </div>
          );
        })}
        {list.length === 0 && (
          <div style={{ color: C.muted, fontSize: 13, textAlign: "center", marginTop: 20 }}>Nenhum exercício encontrado.</div>
        )}
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Progress                                                              */
/* ------------------------------------------------------------------ */
function ProgressScreen({ weeklyVolumeData, bodyWeightData, prMap, bwInput, setBwInput, onAddBodyWeight, unit }) {
  const prList = Object.values(prMap).sort((a, b) => b.weight - a.weight);
  return (
    <div>
      <div style={{ fontFamily: DISPLAY_FONT, fontSize: 30, letterSpacing: 0.5 }}>PROGRESSO</div>

      <div style={{ marginTop: 16, fontWeight: 800, fontSize: 14, display: "flex", alignItems: "center", gap: 6 }}>
        <Zap size={15} color={C.accent} /> Volume semanal (kg)
      </div>
      <div style={{ background: C.surface, border: `1px solid ${C.border}`, borderRadius: 14, padding: "12px 8px 4px", marginTop: 8, height: 180 }}>
        {weeklyVolumeData.length === 0 ? (
          <div style={{ color: C.muted, fontSize: 13, textAlign: "center", paddingTop: 60 }}>Complete treinos para ver seu progresso.</div>
        ) : (
          <ResponsiveContainer width="100%" height="100%">
            <BarChart data={weeklyVolumeData}>
              <CartesianGrid strokeDasharray="3 3" stroke={C.border} vertical={false} />
              <XAxis dataKey="label" stroke={C.muted} fontSize={11} tickLine={false} axisLine={false} />
              <YAxis stroke={C.muted} fontSize={11} tickLine={false} axisLine={false} width={34} />
              <Tooltip contentStyle={{ background: C.surface2, border: `1px solid ${C.border}`, borderRadius: 8, fontSize: 12 }} />
              <Bar dataKey="volume" fill={C.accent} radius={[4, 4, 0, 0]} />
            </BarChart>
          </ResponsiveContainer>
        )}
      </div>

      <div style={{ marginTop: 22, fontWeight: 800, fontSize: 14, display: "flex", alignItems: "center", gap: 6 }}>
        <Award size={15} color={C.accent} /> Recordes pessoais
      </div>
      <div style={{ display: "flex", flexDirection: "column", gap: 8, marginTop: 8 }}>
        {prList.length === 0 && <div style={{ color: C.muted, fontSize: 13 }}>Nenhum recorde ainda.</div>}
        {prList.slice(0, 8).map((p, i) => (
          <div key={i} style={{
            background: C.surface, border: `1px solid ${C.border}`, borderRadius: 12,
            padding: "10px 14px", display: "flex", justifyContent: "space-between", alignItems: "center", gap: 10,
          }}>
            <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
              <ExerciseBadge muscle={EXERCISES.find((e) => e.name === p.name)?.muscle} size={30} />
              <div style={{ fontWeight: 700, fontSize: 13 }}>{p.name}</div>
            </div>
            <div style={{ fontFamily: DISPLAY_FONT, fontSize: 17, color: C.accent }}>{p.weight}{unit} × {p.reps}</div>
          </div>
        ))}
      </div>

      <div style={{ marginTop: 22, fontWeight: 800, fontSize: 14, display: "flex", alignItems: "center", gap: 6 }}>
        <Scale size={15} color={C.accent} /> Peso corporal
      </div>
      <div style={{ display: "flex", gap: 8, marginTop: 8 }}>
        <input value={bwInput} onChange={(e) => setBwInput(e.target.value)} inputMode="decimal"
          placeholder={`Peso de hoje (${unit})`}
          style={{
            flex: 1, background: C.surface, border: `1px solid ${C.border}`, borderRadius: 10,
            padding: "10px 12px", color: C.text, fontSize: 14,
          }} />
        <button onClick={onAddBodyWeight} style={{
          background: C.accent, border: "none", borderRadius: 10, padding: "0 16px",
          color: "#111", fontWeight: 800, fontSize: 13,
        }}>Salvar</button>
      </div>
      <div style={{ background: C.surface, border: `1px solid ${C.border}`, borderRadius: 14, padding: "12px 8px 4px", marginTop: 10, height: 160 }}>
        {bodyWeightData.length < 2 ? (
          <div style={{ color: C.muted, fontSize: 13, textAlign: "center", paddingTop: 50 }}>Registre pelo menos 2 pesagens para ver o gráfico.</div>
        ) : (
          <ResponsiveContainer width="100%" height="100%">
            <LineChart data={bodyWeightData}>
              <CartesianGrid strokeDasharray="3 3" stroke={C.border} vertical={false} />
              <XAxis dataKey="label" stroke={C.muted} fontSize={11} tickLine={false} axisLine={false} />
              <YAxis stroke={C.muted} fontSize={11} tickLine={false} axisLine={false} width={34} domain={["dataMin - 2", "dataMax + 2"]} />
              <Tooltip contentStyle={{ background: C.surface2, border: `1px solid ${C.border}`, borderRadius: 8, fontSize: 12 }} />
              <Line type="monotone" dataKey="peso" stroke={C.accent} strokeWidth={2.5} dot={{ r: 3, fill: C.accent }} />
            </LineChart>
          </ResponsiveContainer>
        )}
      </div>
      <div style={{ height: 20 }} />
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* History                                                               */
/* ------------------------------------------------------------------ */
function HistoryScreen({ history, expanded, setExpanded, onDelete, unit }) {
  const sorted = [...history].sort((a, b) => new Date(b.date) - new Date(a.date));
  return (
    <div>
      <div style={{ fontFamily: DISPLAY_FONT, fontSize: 30, letterSpacing: 0.5 }}>HISTÓRICO</div>
      <div style={{ color: C.muted, fontSize: 13, marginTop: 4 }}>{history.length} treinos registrados</div>

      <div style={{ display: "flex", flexDirection: "column", gap: 10, marginTop: 16 }}>
        {sorted.length === 0 && (
          <div style={{ color: C.muted, fontSize: 13, textAlign: "center", marginTop: 30, lineHeight: 1.5 }}>
            <CalendarDays size={28} color={C.muted} style={{ marginBottom: 8 }} />
            <div>Seus treinos concluídos aparecerão aqui.</div>
          </div>
        )}
        {sorted.map((w) => {
          const isOpen = expanded === w.id;
          return (
            <div key={w.id} style={{ background: C.surface, border: `1px solid ${C.border}`, borderRadius: 14, overflow: "hidden" }}>
              <button onClick={() => setExpanded(isOpen ? null : w.id)} style={{
                width: "100%", display: "flex", justifyContent: "space-between", alignItems: "center",
                padding: "14px", background: "none", border: "none", textAlign: "left",
              }}>
                <div>
                  <div style={{ fontWeight: 800, fontSize: 15 }}>{w.name}</div>
                  <div style={{ color: C.muted, fontSize: 12, marginTop: 2, textTransform: "capitalize" }}>{fmtFullDate(w.date)} · {fmtDuration(w.duration)}</div>
                </div>
                <div style={{ textAlign: "right", display: "flex", alignItems: "center", gap: 10 }}>
                  <div>
                    <div style={{ fontFamily: DISPLAY_FONT, fontSize: 18, color: C.accent }}>{Math.round(w.totalVolume)}</div>
                    <div style={{ color: C.muted, fontSize: 10 }}>kg</div>
                  </div>
                  {isOpen ? <ChevronUp size={16} color={C.muted} /> : <ChevronDown size={16} color={C.muted} />}
                </div>
              </button>
              {isOpen && (
                <div style={{ padding: "0 14px 14px", borderTop: `1px solid ${C.border}` }}>
                  {w.exercises.map((e, i) => (
                    <div key={i} style={{ marginTop: 10 }}>
                      <div style={{ fontWeight: 700, fontSize: 13 }}>{e.name}</div>
                      <div style={{ color: C.muted, fontSize: 12, marginTop: 3 }}>
                        {e.sets.map((s, si) => `${s.weight}${unit}×${s.reps}`).join("  ·  ")}
                      </div>
                    </div>
                  ))}
                  <button onClick={() => onDelete(w.id)} style={{
                    marginTop: 14, width: "100%", padding: "9px 0", borderRadius: 8,
                    background: "rgba(255,90,60,.1)", border: `1px solid ${C.danger}`,
                    color: C.danger, fontSize: 12, fontWeight: 700,
                    display: "flex", alignItems: "center", justifyContent: "center", gap: 6,
                  }}>
                    <Trash2 size={13} /> EXCLUIR TREINO
                  </button>
                </div>
              )}
            </div>
          );
        })}
      </div>
      <div style={{ height: 20 }} />
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Summary overlay (after finishing workout)                            */
/* ------------------------------------------------------------------ */
function SummaryOverlay({ summary, onClose }) {
  const totalSets = summary.exercises.reduce((a, e) => a + e.sets.length, 0);
  return (
    <div style={{ position: "absolute", inset: 0, background: C.bg, zIndex: 80, display: "flex", flexDirection: "column" }}>
      <div style={{ flex: 1, overflowY: "auto", padding: "36px 20px 20px", textAlign: "center" }}>
        <div style={{
          width: 64, height: 64, borderRadius: 20, background: "rgba(200,255,61,.12)",
          display: "flex", alignItems: "center", justifyContent: "center", margin: "0 auto",
        }}>
          <Check size={30} color={C.accent} />
        </div>
        <div style={{ fontFamily: DISPLAY_FONT, fontSize: 32, marginTop: 16, letterSpacing: 0.5 }}>TREINO CONCLUÍDO</div>
        <div style={{ color: C.muted, fontSize: 13, marginTop: 4 }}>{summary.name}</div>

        <div style={{ display: "flex", gap: 10, marginTop: 22 }}>
          {[
            { label: "Volume", value: `${Math.round(summary.totalVolume)}kg` },
            { label: "Duração", value: fmtDuration(summary.duration) },
            { label: "Séries", value: totalSets },
          ].map((s) => (
            <div key={s.label} style={{ flex: 1, background: C.surface, border: `1px solid ${C.border}`, borderRadius: 14, padding: "14px 6px" }}>
              <div style={{ fontFamily: DISPLAY_FONT, fontSize: 22 }}>{s.value}</div>
              <div style={{ color: C.muted, fontSize: 10.5, fontWeight: 700, textTransform: "uppercase" }}>{s.label}</div>
            </div>
          ))}
        </div>

        {summary.prs.length > 0 && (
          <div style={{ marginTop: 24, textAlign: "left" }}>
            <div style={{ display: "flex", alignItems: "center", gap: 6, fontWeight: 800, fontSize: 14 }}>
              <Award size={16} color={C.accent} /> Novos recordes!
            </div>
            <div style={{ display: "flex", flexDirection: "column", gap: 8, marginTop: 10 }}>
              {summary.prs.map((p, i) => (
                <div key={i} style={{
                  background: "rgba(200,255,61,.08)", border: `1px solid ${C.accent}`, borderRadius: 12,
                  padding: "10px 14px", display: "flex", justifyContent: "space-between",
                }}>
                  <div style={{ fontWeight: 700, fontSize: 13 }}>{p.name}</div>
                  <div style={{ fontWeight: 800, fontSize: 13, color: C.accent }}>{p.weight}kg × {p.reps}</div>
                </div>
              ))}
            </div>
          </div>
        )}
      </div>
      <div style={{ padding: 20 }}>
        <button onClick={onClose} style={{
          width: "100%", padding: "16px 0", borderRadius: 14, background: C.accent,
          border: "none", color: "#111", fontSize: 15, fontWeight: 800,
        }}>
          CONCLUIR
        </button>
      </div>
    </div>
  );
}

/* ------------------------------------------------------------------ */
/* Settings overlay                                                      */
/* ------------------------------------------------------------------ */
function SettingsOverlay({ settings, onChangeUnit, onReset, onClose }) {
  return (
    <div style={{ position: "absolute", inset: 0, background: "rgba(0,0,0,.6)", zIndex: 66, display: "flex", alignItems: "flex-end" }}>
      <div style={{ width: "100%", background: C.surface, borderTop: `1px solid ${C.border}`, borderRadius: "20px 20px 0 0", padding: 20 }}>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center" }}>
          <div style={{ fontFamily: DISPLAY_FONT, fontSize: 24 }}>AJUSTES</div>
          <IconBtn onClick={onClose}><X size={16} /></IconBtn>
        </div>

        <div style={{ marginTop: 20 }}>
          <div style={{ color: C.muted, fontSize: 12, fontWeight: 700, textTransform: "uppercase" }}>Unidade de peso</div>
          <div style={{ display: "flex", gap: 8, marginTop: 8 }}>
            {["kg", "lb"].map((u) => (
              <button key={u} onClick={() => onChangeUnit(u)} style={{
                flex: 1, padding: "10px 0", borderRadius: 10,
                background: settings.unit === u ? C.accent : C.surface2,
                border: `1px solid ${settings.unit === u ? C.accent : C.border}`,
                color: settings.unit === u ? "#111" : C.text, fontWeight: 800,
              }}>{u.toUpperCase()}</button>
            ))}
          </div>
        </div>

        <button onClick={onReset} style={{
          marginTop: 26, width: "100%", padding: "13px 0", borderRadius: 10,
          background: "rgba(255,90,60,.1)", border: `1px solid ${C.danger}`,
          color: C.danger, fontWeight: 700, fontSize: 13,
        }}>Apagar todos os dados</button>
      </div>
    </div>
  );
}
