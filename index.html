import React, { useState, useEffect, useMemo, useCallback } from "react";
import {
  Check, Plus, Trash2, Pencil, X, Sparkles, ChevronDown, ChevronRight,
  Loader2, Archive, ChevronLeft, CalendarDays, ChefHat, PiggyBank,
  ListChecks, BookOpen
} from "lucide-react";
import {
  LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer
} from "recharts";

/* ---------------------------------------------------------------------- */
/* Design tokens                                                          */
/* ---------------------------------------------------------------------- */

const INK = "#262220";
const PAPER = "#F2EEE4";
const PAPER_LINE = "#E2DCC9";
const ACCENTS = {
  habits:  { base: "#3F6B52", soft: "#E3EBE2", strong: "#2C4A3A" },
  cooking: { base: "#C1622D", soft: "#F5E4D6", strong: "#7A3A18" },
  finance: { base: "#A9791F", soft: "#F1E7C8", strong: "#6E4F16" },
};
const WEEKDAYS = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
const PRESETS = [
  { label: "Every day", days: [0, 1, 2, 3, 4, 5, 6] },
  { label: "Weekdays", days: [1, 2, 3, 4, 5] },
  { label: "Mon–Sat", days: [1, 2, 3, 4, 5, 6] },
  { label: "Weekends", days: [0, 6] },
];
const MONTH_START = { year: 2026, month: 6 };
const MONTH_END = { year: 2029, month: 6 };

/* ---------------------------------------------------------------------- */
/* Helpers                                                                 */
/* ---------------------------------------------------------------------- */

function pad(n) { return String(n).padStart(2, "0"); }
function dateStr(d) { return `${d.getFullYear()}-${pad(d.getMonth() + 1)}-${pad(d.getDate())}`; }
function todayStr() { return dateStr(new Date()); }
function parseDate(s) { return new Date(s + "T00:00:00"); }
function monthKey(y, m) { return `${y}-${pad(m)}`; }
function monthLabel(y, m) {
  return new Date(y, m - 1, 1).toLocaleString("default", { month: "long", year: "numeric" });
}
function uid(prefix) { return `${prefix}_${Date.now()}_${Math.floor(Math.random() * 10000)}`; }

function generateMonths() {
  const months = [];
  let y = MONTH_START.year, m = MONTH_START.month;
  while (y < MONTH_END.year || (y === MONTH_END.year && m <= MONTH_END.month)) {
    months.push({ year: y, month: m });
    m++;
    if (m > 12) { m = 1; y++; }
  }
  return months;
}
const ALL_MONTHS = generateMonths();

async function safeGet(key) {
  try { return await window.storage.get(key); } catch (e) { return null; }
}
async function safeSet(key, value) {
  try { return await window.storage.set(key, value); } catch (e) { console.error("storage set failed", key, e); return null; }
}

function addDays(d, n) { const r = new Date(d); r.setDate(r.getDate() + n); return r; }
function startOfWeek(d) { const r = new Date(d); r.setDate(r.getDate() - r.getDay()); return r; }
function startOfToday() { const d = new Date(); d.setHours(0, 0, 0, 0); return d; }

function isHabitScheduled(habit, ds, weekday) {
  if (weekday === undefined) weekday = parseDate(ds).getDay();
  if (!habit.days.includes(weekday)) return false;
  if (ds < habit.startDate) return false;
  if (habit.archivedAt && ds >= habit.archivedAt) return false;
  return true;
}

// per-habit scheduled/done/pct across a date range, days after today excluded
function computeRangeStats(habits, completions, startObj, endObj) {
  const today = startOfToday();
  const cappedEnd = endObj > today ? today : endObj;
  const results = habits.map((h) => ({ habit: h, scheduled: 0, done: 0 }));
  for (let d = new Date(startObj); d <= cappedEnd; d = addDays(d, 1)) {
    const ds = dateStr(d);
    const wd = d.getDay();
    results.forEach((r) => {
      if (isHabitScheduled(r.habit, ds, wd)) {
        r.scheduled++;
        if (completions[`${r.habit.id}|${ds}`]) r.done++;
      }
    });
  }
  return results
    .map((r) => ({ ...r, pct: r.scheduled ? Math.round((r.done / r.scheduled) * 100) : null }))
    .filter((r) => r.scheduled > 0);
}

// per-weekday scheduled/done/pct for a single habit across a date range
function computeWeekdayBreakdown(habit, completions, startObj, endObj) {
  const today = startOfToday();
  const cappedEnd = endObj > today ? today : endObj;
  const buckets = Array.from({ length: 7 }, () => ({ scheduled: 0, done: 0 }));
  for (let d = new Date(startObj); d <= cappedEnd; d = addDays(d, 1)) {
    const ds = dateStr(d);
    const wd = d.getDay();
    if (isHabitScheduled(habit, ds, wd)) {
      buckets[wd].scheduled++;
      if (completions[`${habit.id}|${ds}`]) buckets[wd].done++;
    }
  }
  return buckets.map((b, i) => ({ weekday: i, ...b, pct: b.scheduled ? Math.round((b.done / b.scheduled) * 100) : null }));
}

function buildTrendSentence(habit, breakdown) {
  const active = breakdown.filter((b) => b.pct !== null);
  if (active.length < 2) return null;
  const sorted = [...active].sort((a, b) => a.pct - b.pct);
  const worst = sorted[0];
  const best = sorted[sorted.length - 1];
  if (worst.pct === best.pct) return `${habit.name}: rock solid every scheduled day this month (${worst.pct}%).`;
  return `${habit.name}: ${WEEKDAYS[worst.weekday]}s were the struggle (${worst.pct}%) — ${WEEKDAYS[best.weekday]}s you nailed it (${best.pct}%).`;
}

/* ---------------------------------------------------------------------- */
/* Small UI atoms                                                          */
/* ---------------------------------------------------------------------- */

function Card({ children, className = "" }) {
  return (
    <div className={`bg-white/70 rounded-xl border border-black/5 shadow-sm ${className}`}>
      {children}
    </div>
  );
}

function SectionTitle({ children, accent }) {
  return (
    <h2
      className="font-display text-xl tracking-tight mb-3"
      style={{ color: accent.strong }}
    >
      {children}
    </h2>
  );
}

function ProgressBar({ pct, accent }) {
  const clamped = Math.max(0, Math.min(100, pct));
  return (
    <div className="w-full h-2.5 rounded-full bg-black/10 overflow-hidden">
      <div
        className="h-full rounded-full transition-all duration-500"
        style={{ width: `${clamped}%`, backgroundColor: accent.base }}
      />
    </div>
  );
}

function TickStamp({ checked, onClick, accent, size = "md" }) {
  const dim = size === "lg" ? "w-11 h-11" : "w-9 h-9";
  return (
    <button
      onClick={onClick}
      aria-pressed={checked}
      aria-label={checked ? "Mark as not done" : "Mark as done"}
      className={`${dim} rounded-lg border-2 flex items-center justify-center transition-all active:scale-90 shrink-0`}
      style={{
        borderColor: checked ? accent.base : "#C9C2AE",
        backgroundColor: checked ? accent.base : "transparent",
        transform: checked ? "rotate(-2deg)" : "none",
      }}
    >
      {checked && <Check className="w-5 h-5 text-white" strokeWidth={3} />}
    </button>
  );
}

function StatRow({ name, pct, scheduled, done, accent }) {
  return (
    <div className="py-1.5">
      <div className="flex items-baseline justify-between mb-1">
        <span className="text-sm font-medium truncate">{name}</span>
        <span className="font-mono text-xs text-black/40 shrink-0 ml-2">{done}/{scheduled} · {pct}%</span>
      </div>
      <ProgressBar pct={pct} accent={accent} />
    </div>
  );
}

function TextField({ label, ...props }) {
  return (
    <label className="block">
      {label && <span className="text-xs font-medium text-black/50 block mb-1">{label}</span>}
      <input
        {...props}
        className={`w-full px-3 py-2 rounded-lg border border-black/15 bg-white font-body text-sm focus:outline-none focus:ring-2 focus:ring-offset-0 ${props.className || ""}`}
      />
    </label>
  );
}

/* ---------------------------------------------------------------------- */
/* Root App                                                                 */
/* ---------------------------------------------------------------------- */

export default function App() {
  const [tab, setTab] = useState("habits");
  const [loading, setLoading] = useState(true);
  const [habitsData, setHabitsData] = useState({ habits: [], completions: {} });
  const [cookingData, setCookingData] = useState({ skills: [], plans: {} });
  const [financeData, setFinanceData] = useState({
    settings: { goal: 5000, targetDate: "2029-06-17", startDate: "2026-06-17", payFrequency: "monthly", currency: "$", startingBalance: 0 },
    contributions: [],
  });

  useEffect(() => {
    (async () => {
      const [h, c, f] = await Promise.all([
        safeGet("habits-data"), safeGet("cooking-data"), safeGet("finance-data"),
      ]);
      if (h && h.value) { try { setHabitsData(JSON.parse(h.value)); } catch (e) {} }
      if (c && c.value) { try { setCookingData(JSON.parse(c.value)); } catch (e) {} }
      if (f && f.value) { try { setFinanceData(JSON.parse(f.value)); } catch (e) {} }
      setLoading(false);
    })();
  }, []);

  const saveHabits = useCallback((data) => { setHabitsData(data); safeSet("habits-data", JSON.stringify(data)); }, []);
  const saveCooking = useCallback((data) => { setCookingData(data); safeSet("cooking-data", JSON.stringify(data)); }, []);
  const saveFinance = useCallback((data) => { setFinanceData(data); safeSet("finance-data", JSON.stringify(data)); }, []);

  const TABS = [
    { id: "habits", label: "Habits", icon: ListChecks, accent: ACCENTS.habits },
    { id: "cooking", label: "Cooking", icon: ChefHat, accent: ACCENTS.cooking },
    { id: "finance", label: "Savings", icon: PiggyBank, accent: ACCENTS.finance },
  ];
  const activeAccent = TABS.find((t) => t.id === tab).accent;

  return (
    <div
      className="min-h-screen w-full font-body"
      style={{ backgroundColor: PAPER, color: INK }}
    >
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,500;9..144,600;9..144,700&family=Space+Mono:wght@400;700&family=Inter:wght@400;500;600&display=swap');
        .font-display { font-family: 'Fraunces', serif; }
        .font-mono { font-family: 'Space Mono', monospace; }
        .font-body { font-family: 'Inter', sans-serif; }
        input:focus, textarea:focus, select:focus { box-shadow: 0 0 0 2px ${activeAccent.base}55; border-color: ${activeAccent.base}; }
      `}</style>

      <header className="px-4 pt-5 pb-3 sticky top-0 z-20" style={{ backgroundColor: PAPER }}>
        <div className="flex items-baseline justify-between max-w-2xl mx-auto">
          <h1 className="font-display text-2xl font-semibold tracking-tight">Three-Year Almanac</h1>
          <span className="font-mono text-xs text-black/40">{todayStr()}</span>
        </div>
      </header>

      <nav className="px-4 max-w-2xl mx-auto">
        <div className="flex gap-1 border-b border-black/10">
          {TABS.map((t) => {
            const Icon = t.icon;
            const active = tab === t.id;
            return (
              <button
                key={t.id}
                onClick={() => setTab(t.id)}
                className="flex-1 flex items-center justify-center gap-1.5 py-2.5 rounded-t-lg text-sm font-medium transition-colors relative -mb-px"
                style={{
                  backgroundColor: active ? t.accent.soft : "transparent",
                  color: active ? t.accent.strong : "#8a8275",
                  borderTop: active ? `2px solid ${t.accent.base}` : "2px solid transparent",
                  borderLeft: active ? "1px solid rgba(0,0,0,0.06)" : "none",
                  borderRight: active ? "1px solid rgba(0,0,0,0.06)" : "none",
                }}
              >
                <Icon className="w-4 h-4" />
                {t.label}
              </button>
            );
          })}
        </div>
      </nav>

      <main className="px-4 pb-16 pt-5 max-w-2xl mx-auto">
        {loading ? (
          <div className="flex items-center justify-center py-24 text-black/40 gap-2">
            <Loader2 className="w-4 h-4 animate-spin" /> Loading your almanac…
          </div>
        ) : (
          <>
            {tab === "habits" && <HabitsTab data={habitsData} onSave={saveHabits} accent={ACCENTS.habits} />}
            {tab === "cooking" && <CookingTab data={cookingData} onSave={saveCooking} accent={ACCENTS.cooking} />}
            {tab === "finance" && <FinanceTab data={financeData} onSave={saveFinance} accent={ACCENTS.finance} />}
          </>
        )}
      </main>
    </div>
  );
}

/* ---------------------------------------------------------------------- */
/* HABITS TAB                                                              */
/* ---------------------------------------------------------------------- */

function HabitsTab({ data, onSave, accent }) {
  const [viewDate, setViewDate] = useState(todayStr());
  const [showManage, setShowManage] = useState(data.habits.length === 0);
  const [newName, setNewName] = useState("");
  const [newDays, setNewDays] = useState([1, 2, 3, 4, 5, 6]);
  const [editingId, setEditingId] = useState(null);
  const [weekAnchor, setWeekAnchor] = useState(todayStr());
  const [monthAnchor, setMonthAnchor] = useState(`${new Date().getFullYear()}-${pad(new Date().getMonth() + 1)}`);

  const weekStartObj = startOfWeek(parseDate(weekAnchor));
  const weekEndObj = addDays(weekStartObj, 6);
  const weeklyStats = useMemo(
    () => computeRangeStats(data.habits, data.completions, weekStartObj, weekEndObj),
    [data.habits, data.completions, weekAnchor]
  );
  const thisWeekStart = startOfWeek(startOfToday());
  const canGoNextWeek = weekStartObj < thisWeekStart;

  const [monthYearStr, monthNumStr] = monthAnchor.split("-");
  const monthYear = Number(monthYearStr), monthNum = Number(monthNumStr);
  const monthStartObj = new Date(monthYear, monthNum - 1, 1);
  const monthEndObj = new Date(monthYear, monthNum, 0);
  const monthlyStats = useMemo(
    () => computeRangeStats(data.habits, data.completions, monthStartObj, monthEndObj),
    [data.habits, data.completions, monthAnchor]
  );
  const trendSentences = useMemo(
    () => data.habits
      .map((h) => buildTrendSentence(h, computeWeekdayBreakdown(h, data.completions, monthStartObj, monthEndObj)))
      .filter(Boolean),
    [data.habits, data.completions, monthAnchor]
  );
  const today = new Date(); today.setHours(0, 0, 0, 0);
  const thisMonthKey = `${today.getFullYear()}-${pad(today.getMonth() + 1)}`;
  const canGoNextMonth = monthAnchor < thisMonthKey;

  function shiftWeek(weeks) { setWeekAnchor(dateStr(addDays(weekStartObj, weeks * 7))); }
  function shiftMonth(delta) {
    const d = new Date(monthYear, monthNum - 1 + delta, 1);
    setMonthAnchor(`${d.getFullYear()}-${pad(d.getMonth() + 1)}`);
  }

  const weekday = parseDate(viewDate).getDay();
  const activeHabits = data.habits.filter((h) => {
    if (h.archivedAt && viewDate >= h.archivedAt) return false;
    if (viewDate < h.startDate) return false;
    return h.days.includes(weekday);
  });

  function toggleDay(arr, d) {
    return arr.includes(d) ? arr.filter((x) => x !== d) : [...arr, d].sort();
  }

  function addHabit() {
    if (!newName.trim() || newDays.length === 0) return;
    const habit = { id: uid("h"), name: newName.trim(), days: newDays, startDate: todayStr(), archivedAt: null };
    onSave({ ...data, habits: [...data.habits, habit] });
    setNewName(""); setNewDays([1, 2, 3, 4, 5, 6]);
  }

  function updateHabit(id, updates) {
    onSave({ ...data, habits: data.habits.map((h) => (h.id === id ? { ...h, ...updates } : h)) });
  }

  function archiveHabit(id) {
    updateHabit(id, { archivedAt: todayStr() });
  }

  function deleteHabit(id) {
    onSave({ ...data, habits: data.habits.filter((h) => h.id !== id) });
  }

  function toggleCompletion(habitId) {
    const key = `${habitId}|${viewDate}`;
    const completions = { ...data.completions };
    if (completions[key]) delete completions[key];
    else completions[key] = true;
    onSave({ ...data, completions });
  }

  function shiftDate(deltaDays) {
    const d = parseDate(viewDate);
    d.setDate(d.getDate() + deltaDays);
    setViewDate(dateStr(d));
  }

  // last 7 day dots per habit
  function recentDots(habit) {
    const dots = [];
    for (let i = 6; i >= 0; i--) {
      const d = new Date();
      d.setDate(d.getDate() - i);
      const ds = dateStr(d);
      const wd = d.getDay();
      const applies = habit.days.includes(wd) && ds >= habit.startDate && (!habit.archivedAt || ds < habit.archivedAt);
      const done = !!data.completions[`${habit.id}|${ds}`];
      dots.push({ applies, done, key: ds });
    }
    return dots;
  }

  const isToday = viewDate === todayStr();

  return (
    <div className="space-y-6">
      <Card className="p-4">
        <div className="flex items-center justify-between mb-4">
          <button onClick={() => shiftDate(-1)} className="p-1.5 rounded-md hover:bg-black/5"><ChevronLeft className="w-4 h-4" /></button>
          <div className="text-center">
            <div className="font-display text-lg font-semibold">{isToday ? "Today" : WEEKDAYS[weekday] + "day"}</div>
            <div className="font-mono text-xs text-black/40">{viewDate}{isToday ? "" : ""}</div>
          </div>
          <button onClick={() => shiftDate(1)} className="p-1.5 rounded-md hover:bg-black/5"><ChevronRight className="w-4 h-4" /></button>
        </div>

        {activeHabits.length === 0 ? (
          <p className="text-sm text-black/50 text-center py-6">
            {data.habits.length === 0
              ? "No habits yet. Add your first one below — you can set it for every day, just weekdays, or any mix you like."
              : "Nothing scheduled for this day."}
          </p>
        ) : (
          <ul className="space-y-2">
            {activeHabits.map((h) => {
              const done = !!data.completions[`${h.id}|${viewDate}`];
              return (
                <li key={h.id} className="flex items-center gap-3 py-1.5">
                  <TickStamp checked={done} onClick={() => toggleCompletion(h.id)} accent={accent} />
                  <div className="flex-1 min-w-0">
                    <div className={`text-sm font-medium ${done ? "text-black/40 line-through" : ""}`}>{h.name}</div>
                    <div className="flex gap-1 mt-1">
                      {recentDots(h).map((d) => (
                        <span
                          key={d.key}
                          className="w-1.5 h-1.5 rounded-full"
                          style={{ backgroundColor: !d.applies ? "#00000010" : d.done ? accent.base : "#00000025" }}
                        />
                      ))}
                    </div>
                  </div>
                </li>
              );
            })}
          </ul>
        )}
      </Card>

      <Card className="p-4">
        <div className="flex items-center justify-between mb-3">
          <button onClick={() => shiftWeek(-1)} className="p-1.5 rounded-md hover:bg-black/5"><ChevronLeft className="w-4 h-4" /></button>
          <div className="text-center">
            <div className="font-display text-base font-semibold">Weekly check-in</div>
            <div className="font-mono text-xs text-black/40">{dateStr(weekStartObj)} – {dateStr(weekEndObj)}</div>
          </div>
          <button onClick={() => canGoNextWeek && shiftWeek(1)} disabled={!canGoNextWeek} className="p-1.5 rounded-md hover:bg-black/5 disabled:opacity-30"><ChevronRight className="w-4 h-4" /></button>
        </div>
        {weeklyStats.length === 0 ? (
          <p className="text-sm text-black/50 text-center py-4">No data for this week yet.</p>
        ) : (
          <>
            <div className="divide-y divide-black/5">
              {weeklyStats.map((s) => (
                <StatRow key={s.habit.id} name={s.habit.name} pct={s.pct} scheduled={s.scheduled} done={s.done} accent={accent} />
              ))}
            </div>
            {weeklyStats.length > 1 && (() => {
              const worst = [...weeklyStats].sort((a, b) => a.pct - b.pct)[0];
              const best = [...weeklyStats].sort((a, b) => b.pct - a.pct)[0];
              if (worst.pct === best.pct) return null;
              return (
                <p className="text-xs text-black/50 mt-3 pt-3 border-t border-black/10">
                  Most room to grow: <strong>{worst.habit.name}</strong> ({worst.pct}%). Strongest: <strong>{best.habit.name}</strong> ({best.pct}%).
                </p>
              );
            })()}
          </>
        )}
      </Card>

      <Card className="p-4">
        <div className="flex items-center justify-between mb-3">
          <button onClick={() => shiftMonth(-1)} className="p-1.5 rounded-md hover:bg-black/5"><ChevronLeft className="w-4 h-4" /></button>
          <div className="text-center">
            <div className="font-display text-base font-semibold">Monthly trends</div>
            <div className="font-mono text-xs text-black/40">{monthLabel(monthYear, monthNum)}</div>
          </div>
          <button onClick={() => canGoNextMonth && shiftMonth(1)} disabled={!canGoNextMonth} className="p-1.5 rounded-md hover:bg-black/5 disabled:opacity-30"><ChevronRight className="w-4 h-4" /></button>
        </div>
        {monthlyStats.length === 0 ? (
          <p className="text-sm text-black/50 text-center py-4">No data for this month yet.</p>
        ) : (
          <>
            <div className="divide-y divide-black/5 mb-3">
              {monthlyStats.map((s) => (
                <StatRow key={s.habit.id} name={s.habit.name} pct={s.pct} scheduled={s.scheduled} done={s.done} accent={accent} />
              ))}
            </div>
            <div className="pt-3 border-t border-black/10">
              <div className="text-xs font-medium text-black/50 mb-2">Day-of-week pattern</div>
              {trendSentences.length === 0 ? (
                <p className="text-xs text-black/40">Keep logging through the month to see day-of-week patterns.</p>
              ) : (
                <ul className="space-y-1.5">
                  {trendSentences.map((t, i) => (
                    <li key={i} className="text-xs text-black/60">{t}</li>
                  ))}
                </ul>
              )}
            </div>
          </>
        )}
      </Card>

      <div>
        <button
          onClick={() => setShowManage((s) => !s)}
          className="flex items-center gap-1.5 text-sm font-medium mb-3"
          style={{ color: accent.strong }}
        >
          {showManage ? <ChevronDown className="w-4 h-4" /> : <ChevronRight className="w-4 h-4" />}
          Manage habits
        </button>

        {showManage && (
          <Card className="p-4 space-y-4">
            <div>
              <SectionTitle accent={accent}>Add a habit</SectionTitle>
              <div className="space-y-3">
                <TextField placeholder="e.g. Drink 2L water" value={newName} onChange={(e) => setNewName(e.target.value)} />
                <div>
                  <div className="flex gap-1.5 flex-wrap mb-2">
                    {PRESETS.map((p) => (
                      <button
                        key={p.label}
                        onClick={() => setNewDays(p.days)}
                        className="text-xs px-2.5 py-1 rounded-full border border-black/15 hover:bg-black/5"
                      >
                        {p.label}
                      </button>
                    ))}
                  </div>
                  <div className="flex gap-1">
                    {WEEKDAYS.map((wd, i) => (
                      <button
                        key={wd}
                        onClick={() => setNewDays((d) => toggleDay(d, i))}
                        className="w-9 h-9 rounded-lg text-xs font-medium border"
                        style={{
                          backgroundColor: newDays.includes(i) ? accent.base : "white",
                          color: newDays.includes(i) ? "white" : INK,
                          borderColor: newDays.includes(i) ? accent.base : "#00000020",
                        }}
                      >
                        {wd[0]}
                      </button>
                    ))}
                  </div>
                </div>
                <button
                  onClick={addHabit}
                  disabled={!newName.trim() || newDays.length === 0}
                  className="flex items-center gap-1.5 px-3 py-2 rounded-lg text-sm font-medium text-white disabled:opacity-40"
                  style={{ backgroundColor: accent.base }}
                >
                  <Plus className="w-4 h-4" /> Add habit
                </button>
              </div>
            </div>

            {data.habits.length > 0 && (
              <div className="pt-3 border-t border-black/10">
                <SectionTitle accent={accent}>Your habits</SectionTitle>
                <ul className="space-y-2">
                  {data.habits.map((h) => (
                    <HabitRow
                      key={h.id}
                      habit={h}
                      accent={accent}
                      editing={editingId === h.id}
                      onEdit={() => setEditingId(editingId === h.id ? null : h.id)}
                      onUpdate={(u) => updateHabit(h.id, u)}
                      onArchive={() => archiveHabit(h.id)}
                      onDelete={() => deleteHabit(h.id)}
                      toggleDay={toggleDay}
                    />
                  ))}
                </ul>
                <p className="text-xs text-black/40 mt-2">
                  Changing your routine? Edit a habit's days any time, or archive it to stop tracking while keeping its history.
                </p>
              </div>
            )}
          </Card>
        )}
      </div>
    </div>
  );
}

function HabitRow({ habit, accent, editing, onEdit, onUpdate, onArchive, onDelete, toggleDay }) {
  const isArchived = !!habit.archivedAt;
  return (
    <li className="rounded-lg border border-black/10 p-3" style={{ opacity: isArchived ? 0.5 : 1 }}>
      <div className="flex items-center justify-between gap-2">
        <div className="min-w-0">
          <div className="text-sm font-medium truncate">{habit.name}{isArchived ? " (archived)" : ""}</div>
          <div className="text-xs text-black/40">{habit.days.map((d) => WEEKDAYS[d]).join(", ")}</div>
        </div>
        <div className="flex items-center gap-1 shrink-0">
          <button onClick={onEdit} className="p-1.5 rounded-md hover:bg-black/5"><Pencil className="w-3.5 h-3.5" /></button>
          {!isArchived && <button onClick={onArchive} className="p-1.5 rounded-md hover:bg-black/5"><Archive className="w-3.5 h-3.5" /></button>}
          <button onClick={onDelete} className="p-1.5 rounded-md hover:bg-black/5"><Trash2 className="w-3.5 h-3.5 text-red-600/70" /></button>
        </div>
      </div>
      {editing && (
        <div className="mt-3 pt-3 border-t border-black/10 space-y-2">
          <TextField
            value={habit.name}
            onChange={(e) => onUpdate({ name: e.target.value })}
          />
          <div className="flex gap-1">
            {WEEKDAYS.map((wd, i) => (
              <button
                key={wd}
                onClick={() => onUpdate({ days: toggleDay(habit.days, i) })}
                className="w-8 h-8 rounded-md text-xs font-medium border"
                style={{
                  backgroundColor: habit.days.includes(i) ? accent.base : "white",
                  color: habit.days.includes(i) ? "white" : INK,
                  borderColor: habit.days.includes(i) ? accent.base : "#00000020",
                }}
              >
                {wd[0]}
              </button>
            ))}
          </div>
        </div>
      )}
    </li>
  );
}

/* ---------------------------------------------------------------------- */
/* COOKING TAB                                                             */
/* ---------------------------------------------------------------------- */

function CookingTab({ data, onSave, accent }) {
  const [showSkills, setShowSkills] = useState(data.skills.length === 0);
  const [newSkill, setNewSkill] = useState("");
  const [openYear, setOpenYear] = useState(MONTH_START.year);

  function addSkill() {
    if (!newSkill.trim()) return;
    onSave({ ...data, skills: [...data.skills, { id: uid("s"), name: newSkill.trim() }] });
    setNewSkill("");
  }
  function deleteSkill(id) {
    onSave({ ...data, skills: data.skills.filter((s) => s.id !== id) });
  }

  function autoFill() {
    if (data.skills.length === 0) return;
    const plans = { ...data.plans };
    let i = 0;
    ALL_MONTHS.forEach(({ year, month }) => {
      const key = monthKey(year, month);
      if (!plans[key] || !plans[key].skillId) {
        const skill = data.skills[i % data.skills.length];
        plans[key] = { ...(plans[key] || emptyPlan()), skillId: skill.id };
        i++;
      }
    });
    onSave({ ...data, plans });
  }

  function emptyPlan() {
    return { skillId: "", customName: "", plannedDate: "", notes: "", recipes: [], completed: false };
  }

  function commitPlan(key, updates) {
    const plans = { ...data.plans };
    plans[key] = { ...(plans[key] || emptyPlan()), ...updates };
    onSave({ ...data, plans });
  }

  const years = [...new Set(ALL_MONTHS.map((m) => m.year))];
  const monthsByYear = (y) => ALL_MONTHS.filter((m) => m.year === y);
  const completedCount = Object.values(data.plans).filter((p) => p.completed).length;

  return (
    <div className="space-y-6">
      <Card className="p-4">
        <div className="flex items-center justify-between mb-1">
          <SectionTitle accent={accent}>Cooking plan</SectionTitle>
          <span className="font-mono text-xs text-black/40">{completedCount}/{ALL_MONTHS.length} sessions logged</span>
        </div>
        <ProgressBar pct={(completedCount / ALL_MONTHS.length) * 100} accent={accent} />
        <p className="text-xs text-black/50 mt-2">June 2026 – June 2029 · one cooking skill to focus on each month.</p>
      </Card>

      <div>
        <button
          onClick={() => setShowSkills((s) => !s)}
          className="flex items-center gap-1.5 text-sm font-medium mb-3"
          style={{ color: accent.strong }}
        >
          {showSkills ? <ChevronDown className="w-4 h-4" /> : <ChevronRight className="w-4 h-4" />}
          Things you want to learn to cook
        </button>
        {showSkills && (
          <Card className="p-4 space-y-3">
            {data.skills.length === 0 && (
              <p className="text-sm text-black/50">List the basics you want to learn — e.g. bread, knife skills, stock. I'll spread them across the next three years and suggest recipes for each.</p>
            )}
            <div className="flex gap-2">
              <TextField placeholder="e.g. Homemade bread" value={newSkill} onChange={(e) => setNewSkill(e.target.value)}
                onKeyDown={(e) => e.key === "Enter" && addSkill()} />
              <button onClick={addSkill} disabled={!newSkill.trim()} className="px-3 rounded-lg text-white disabled:opacity-40 shrink-0" style={{ backgroundColor: accent.base }}>
                <Plus className="w-4 h-4" />
              </button>
            </div>
            {data.skills.length > 0 && (
              <ul className="flex flex-wrap gap-2">
                {data.skills.map((s) => (
                  <li key={s.id} className="flex items-center gap-1.5 text-sm px-3 py-1.5 rounded-full" style={{ backgroundColor: accent.soft, color: accent.strong }}>
                    {s.name}
                    <button onClick={() => deleteSkill(s.id)}><X className="w-3 h-3" /></button>
                  </li>
                ))}
              </ul>
            )}
            {data.skills.length > 0 && (
              <button onClick={autoFill} className="text-sm font-medium underline" style={{ color: accent.strong }}>
                Auto-fill empty months by cycling through this list
              </button>
            )}
          </Card>
        )}
      </div>

      <div className="space-y-3">
        {years.map((y) => (
          <div key={y}>
            <button
              onClick={() => setOpenYear(openYear === y ? null : y)}
              className="w-full flex items-center justify-between px-1 py-2 font-display text-lg font-semibold"
            >
              <span>{y}</span>
              {openYear === y ? <ChevronDown className="w-5 h-5" /> : <ChevronRight className="w-5 h-5" />}
            </button>
            {openYear === y && (
              <div className="space-y-3">
                {monthsByYear(y).map(({ year, month }) => {
                  const key = monthKey(year, month);
                  return (
                    <MonthCard
                      key={key}
                      label={monthLabel(year, month)}
                      plan={data.plans[key] || emptyPlan()}
                      skills={data.skills}
                      accent={accent}
                      onCommit={(updates) => commitPlan(key, updates)}
                    />
                  );
                })}
              </div>
            )}
          </div>
        ))}
      </div>
    </div>
  );
}

function MonthCard({ label, plan, skills, accent, onCommit }) {
  const [expanded, setExpanded] = useState(false);
  const [customName, setCustomName] = useState(plan.customName || "");
  const [notes, setNotes] = useState(plan.notes || "");
  const [plannedDate, setPlannedDate] = useState(plan.plannedDate || "");
  const [newRecipe, setNewRecipe] = useState("");
  const [loadingRecipes, setLoadingRecipes] = useState(false);
  const [recipeError, setRecipeError] = useState("");

  const skillName = plan.skillId ? (skills.find((s) => s.id === plan.skillId)?.name || "") : customName;

  function addManualRecipe() {
    if (!newRecipe.trim()) return;
    onCommit({ recipes: [...(plan.recipes || []), { id: uid("r"), name: newRecipe.trim(), description: "" }] });
    setNewRecipe("");
  }
  function removeRecipe(id) {
    onCommit({ recipes: plan.recipes.filter((r) => r.id !== id) });
  }

  async function suggestRecipes() {
    const theme = plan.skillId ? skills.find((s) => s.id === plan.skillId)?.name : customName;
    if (!theme || !theme.trim()) { setRecipeError("Set a skill or theme for this month first."); return; }
    setLoadingRecipes(true);
    setRecipeError("");
    try {
      const res = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-6",
          max_tokens: 1000,
          messages: [{
            role: "user",
            content: `Suggest 4 beginner-friendly recipe ideas for someone practicing the cooking skill/theme "${theme}" at home. Respond with ONLY a raw JSON array, no markdown fences, no preamble, in exactly this format: [{"name":"Recipe name","description":"one short line, under 14 words"}]`,
          }],
        }),
      });
      const json = await res.json();
      const text = (json.content || []).map((b) => b.text || "").join("");
      const clean = text.replace(/```json|```/g, "").trim();
      const ideas = JSON.parse(clean);
      const newRecipes = ideas.map((idea) => ({ id: uid("r"), name: idea.name, description: idea.description || "" }));
      onCommit({ recipes: [...(plan.recipes || []), ...newRecipes] });
    } catch (e) {
      setRecipeError("Couldn't fetch suggestions right now — try again in a moment.");
    } finally {
      setLoadingRecipes(false);
    }
  }

  return (
    <Card className="p-4">
      <div className="flex items-start justify-between gap-3">
        <button onClick={() => setExpanded((e) => !e)} className="flex-1 text-left">
          <div className="flex items-center gap-2">
            <span className="font-display text-base font-semibold">{label}</span>
            {plan.completed && <Check className="w-4 h-4" style={{ color: accent.base }} />}
          </div>
          <div className="text-sm text-black/50 mt-0.5">{skillName ? skillName : "Not planned yet"}</div>
        </button>
        <TickStamp checked={plan.completed} onClick={() => onCommit({ completed: !plan.completed })} accent={accent} />
      </div>

      {expanded && (
        <div className="mt-4 pt-4 border-t border-black/10 space-y-3">
          <label className="block">
            <span className="text-xs font-medium text-black/50 block mb-1">Skill / theme</span>
            <select
              value={plan.skillId || ""}
              onChange={(e) => onCommit({ skillId: e.target.value, customName: e.target.value ? "" : customName })}
              className="w-full px-3 py-2 rounded-lg border border-black/15 bg-white text-sm"
            >
              <option value="">{skills.length ? "— choose from your list —" : "No skills added yet"}</option>
              {skills.map((s) => <option key={s.id} value={s.id}>{s.name}</option>)}
            </select>
          </label>

          {!plan.skillId && (
            <TextField
              label="Or type a custom theme for this month"
              value={customName}
              onChange={(e) => setCustomName(e.target.value)}
              onBlur={() => onCommit({ customName })}
              placeholder="e.g. Try a new cuisine"
            />
          )}

          <TextField
            label="Planned date this month"
            type="date"
            value={plannedDate}
            onChange={(e) => { setPlannedDate(e.target.value); onCommit({ plannedDate: e.target.value }); }}
          />

          <label className="block">
            <span className="text-xs font-medium text-black/50 block mb-1">Notes</span>
            <textarea
              value={notes}
              onChange={(e) => setNotes(e.target.value)}
              onBlur={() => onCommit({ notes })}
              rows={2}
              className="w-full px-3 py-2 rounded-lg border border-black/15 bg-white text-sm"
              placeholder="Who's coming, what to prep ahead…"
            />
          </label>

          <div>
            <div className="flex items-center justify-between mb-2">
              <span className="text-xs font-medium text-black/50">Recipe ideas</span>
              <button
                onClick={suggestRecipes}
                disabled={loadingRecipes}
                className="flex items-center gap-1 text-xs font-medium px-2.5 py-1 rounded-full disabled:opacity-50"
                style={{ backgroundColor: accent.soft, color: accent.strong }}
              >
                {loadingRecipes ? <Loader2 className="w-3 h-3 animate-spin" /> : <Sparkles className="w-3 h-3" />}
                Suggest recipes
              </button>
            </div>
            {recipeError && <p className="text-xs text-red-600 mb-2">{recipeError}</p>}
            {(plan.recipes || []).length === 0 ? (
              <p className="text-xs text-black/40">No recipe ideas yet — tap "Suggest recipes" or add your own below.</p>
            ) : (
              <ul className="space-y-1.5 mb-2">
                {plan.recipes.map((r) => (
                  <li key={r.id} className="flex items-start gap-2 text-sm bg-black/[0.03] rounded-lg px-3 py-2">
                    <BookOpen className="w-3.5 h-3.5 mt-0.5 shrink-0" style={{ color: accent.base }} />
                    <div className="flex-1 min-w-0">
                      <div className="font-medium">{r.name}</div>
                      {r.description && <div className="text-xs text-black/50">{r.description}</div>}
                    </div>
                    <button onClick={() => removeRecipe(r.id)}><X className="w-3.5 h-3.5 text-black/30" /></button>
                  </li>
                ))}
              </ul>
            )}
            <div className="flex gap-2">
              <TextField placeholder="Add your own idea" value={newRecipe} onChange={(e) => setNewRecipe(e.target.value)}
                onKeyDown={(e) => e.key === "Enter" && addManualRecipe()} />
              <button onClick={addManualRecipe} className="px-3 rounded-lg border border-black/15 shrink-0"><Plus className="w-4 h-4" /></button>
            </div>
          </div>
        </div>
      )}
    </Card>
  );
}

/* ---------------------------------------------------------------------- */
/* FINANCE TAB                                                             */
/* ---------------------------------------------------------------------- */

function FinanceTab({ data, onSave, accent }) {
  const { settings, contributions } = data;
  const [showSettings, setShowSettings] = useState(false);
  const [goal, setGoal] = useState(settings.goal);
  const [targetDate, setTargetDate] = useState(settings.targetDate);
  const [currency, setCurrency] = useState(settings.currency);
  const [payFrequency, setPayFrequency] = useState(settings.payFrequency);
  const [startingBalance, setStartingBalance] = useState(settings.startingBalance || 0);
  const [amount, setAmount] = useState("");
  const [date, setDate] = useState(todayStr());
  const [note, setNote] = useState("");

  function commitSettings(updates) {
    onSave({ ...data, settings: { ...settings, ...updates } });
  }

  const contributedTotal = contributions.reduce((sum, c) => sum + Number(c.amount || 0), 0);
  const totalSaved = Number(settings.startingBalance || 0) + contributedTotal;
  const pct = settings.goal > 0 ? (totalSaved / settings.goal) * 100 : 0;
  const remaining = Math.max(0, settings.goal - totalSaved);

  const lastDate = contributions.length
    ? contributions.reduce((max, c) => (c.date > max ? c.date : max), contributions[0].date)
    : settings.startDate;

  function addPeriod(ds, freq) {
    const d = parseDate(ds);
    if (freq === "weekly") d.setDate(d.getDate() + 7);
    else if (freq === "biweekly") d.setDate(d.getDate() + 14);
    else d.setMonth(d.getMonth() + 1);
    return dateStr(d);
  }
  const nextDue = addPeriod(lastDate, settings.payFrequency);
  const showReminder = todayStr() >= nextDue;

  function addContribution() {
    const amt = Number(amount);
    if (!amt || amt <= 0 || !date) return;
    onSave({
      ...data,
      contributions: [...contributions, { id: uid("c"), amount: amt, date, note: note.trim() }],
    });
    setAmount(""); setNote("");
  }
  function deleteContribution(id) {
    onSave({ ...data, contributions: contributions.filter((c) => c.id !== id) });
  }

  const sorted = [...contributions].sort((a, b) => (a.date < b.date ? -1 : 1));
  const chartData = useMemo(() => {
    const start = parseDate(settings.startDate);
    const end = parseDate(settings.targetDate);
    const totalMonths = Math.max(1, Math.round((end - start) / (1000 * 60 * 60 * 24 * 30.44)));
    const startBal = Number(settings.startingBalance || 0);
    const points = [];
    let runningActual = startBal;
    let ci = 0;
    for (let i = 0; i <= totalMonths; i++) {
      const d = new Date(start);
      d.setMonth(d.getMonth() + i);
      const cutoff = dateStr(d);
      while (ci < sorted.length && sorted[ci].date <= cutoff) {
        runningActual += Number(sorted[ci].amount);
        ci++;
      }
      points.push({
        label: d.toLocaleString("default", { month: "short", year: "2-digit" }),
        actual: Math.round(runningActual),
        target: Math.round(startBal + ((settings.goal - startBal) * i) / totalMonths),
      });
    }
    return points;
  }, [sorted, settings.startDate, settings.targetDate, settings.goal, settings.startingBalance]);

  return (
    <div className="space-y-6">
      <Card className="p-4">
        <SectionTitle accent={accent}>Savings goal</SectionTitle>
        <div className="flex items-baseline justify-between mb-2">
          <span className="font-mono text-2xl font-bold">{currency}{totalSaved.toLocaleString()}</span>
          <span className="text-sm text-black/40">of {currency}{Number(settings.goal).toLocaleString()}</span>
        </div>
        <ProgressBar pct={pct} accent={accent} />
        <div className="flex justify-between text-xs text-black/40 mt-2">
          <span>{pct.toFixed(0)}% there</span>
          <span>{currency}{remaining.toLocaleString()} to go · by {settings.targetDate}</span>
        </div>
        {Number(settings.startingBalance) > 0 && (
          <p className="text-xs text-black/40 mt-1">Includes {currency}{Number(settings.startingBalance).toLocaleString()} already saved before tracking started.</p>
        )}

        <div className="mt-4 h-40">
          <ResponsiveContainer width="100%" height="100%">
            <LineChart data={chartData} margin={{ top: 5, right: 5, left: -20, bottom: 0 }}>
              <CartesianGrid strokeDasharray="3 3" stroke="#00000010" />
              <XAxis dataKey="label" tick={{ fontSize: 10, fill: "#00000060" }} interval={Math.ceil(chartData.length / 6)} />
              <YAxis tick={{ fontSize: 10, fill: "#00000060" }} />
              <Tooltip formatter={(v) => `${currency}${v}`} />
              <Line type="monotone" dataKey="target" stroke="#00000030" strokeDasharray="4 4" dot={false} name="Target pace" />
              <Line type="monotone" dataKey="actual" stroke={accent.base} strokeWidth={2.5} dot={false} name="Actual saved" />
            </LineChart>
          </ResponsiveContainer>
        </div>
      </Card>

      {showReminder && (
        <Card className="p-4" style={{ backgroundColor: accent.soft, borderColor: accent.base }}>
          <p className="text-sm font-medium" style={{ color: accent.strong }}>
            Looks like payday's come around — log this period's contribution below.
          </p>
        </Card>
      )}

      <Card className="p-4">
        <SectionTitle accent={accent}>Log a contribution</SectionTitle>
        <div className="grid grid-cols-2 gap-2 mb-2">
          <TextField label="Amount" type="number" min="0" step="0.01" value={amount} onChange={(e) => setAmount(e.target.value)} placeholder="0.00" />
          <TextField label="Date" type="date" value={date} onChange={(e) => setDate(e.target.value)} />
        </div>
        <TextField label="Note (optional)" value={note} onChange={(e) => setNote(e.target.value)} placeholder="e.g. Payslip — June" />
        <button
          onClick={addContribution}
          disabled={!amount || Number(amount) <= 0}
          className="mt-3 flex items-center gap-1.5 px-3 py-2 rounded-lg text-sm font-medium text-white disabled:opacity-40"
          style={{ backgroundColor: accent.base }}
        >
          <Plus className="w-4 h-4" /> Add contribution
        </button>
      </Card>

      {contributions.length > 0 && (
        <Card className="p-4">
          <SectionTitle accent={accent}>History</SectionTitle>
          <ul className="space-y-1.5 max-h-64 overflow-y-auto">
            {[...sorted].reverse().map((c) => (
              <li key={c.id} className="flex items-center justify-between text-sm py-1.5 border-b border-black/5 last:border-0">
                <div>
                  <span className="font-mono">{currency}{Number(c.amount).toLocaleString()}</span>
                  <span className="text-black/40 ml-2">{c.date}</span>
                  {c.note && <span className="text-black/40 ml-2">· {c.note}</span>}
                </div>
                <button onClick={() => deleteContribution(c.id)}><Trash2 className="w-3.5 h-3.5 text-red-600/60" /></button>
              </li>
            ))}
          </ul>
        </Card>
      )}

      <div>
        <button onClick={() => setShowSettings((s) => !s)} className="flex items-center gap-1.5 text-sm font-medium mb-3" style={{ color: accent.strong }}>
          {showSettings ? <ChevronDown className="w-4 h-4" /> : <ChevronRight className="w-4 h-4" />}
          Goal settings
        </button>
        {showSettings && (
          <Card className="p-4 space-y-3">
            <TextField label="Already saved (starting balance)" type="number" min="0" step="0.01" value={startingBalance} onChange={(e) => setStartingBalance(e.target.value)} onBlur={() => commitSettings({ startingBalance: Number(startingBalance) || 0 })} />
            <TextField label="Savings goal" type="number" min="0" value={goal} onChange={(e) => setGoal(e.target.value)} onBlur={() => commitSettings({ goal: Number(goal) })} />
            <TextField label="Target date" type="date" value={targetDate} onChange={(e) => { setTargetDate(e.target.value); commitSettings({ targetDate: e.target.value }); }} />
            <TextField label="Currency symbol" value={currency} maxLength={3} onChange={(e) => setCurrency(e.target.value)} onBlur={() => commitSettings({ currency })} />
            <label className="block">
              <span className="text-xs font-medium text-black/50 block mb-1">Payday frequency</span>
              <select
                value={payFrequency}
                onChange={(e) => { setPayFrequency(e.target.value); commitSettings({ payFrequency: e.target.value }); }}
                className="w-full px-3 py-2 rounded-lg border border-black/15 bg-white text-sm"
              >
                <option value="weekly">Weekly</option>
                <option value="biweekly">Every 2 weeks</option>
                <option value="monthly">Monthly</option>
              </select>
            </label>
          </Card>
        )}
      </div>
    </div>
  );
}
