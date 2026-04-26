---
name: laws-of-simplicity
description: Apply John Maeda's 10 Laws of Simplicity to dashboard and UI design decisions. Use this skill whenever the user is designing, building, or reviewing a dashboard, SaaS UI, fleet management tool, data visualization, or any web interface. Trigger when the user asks about layout, information hierarchy, alerts, user tasks, onboarding, or what to show on a screen. Also trigger when the user says things like "what should be visible", "how should I organize this", "is this too complex", or "what's most important to show". Always use this skill before writing any dashboard component or layout code.
---

# Laws of Simplicity — Dashboard & UI Design Skill

Based on John Maeda's *The Laws of Simplicity* (MIT Press). Apply these laws when making decisions about what to show, how to organize it, and how to guide the user to action.

---

## The 10 Laws Applied to Dashboards

### Law 1: Reduce
Remove everything that doesn't directly serve a decision or action.

**Ask before adding any element:**
> "If I remove this, does the user make a worse decision?"

If the answer is no — cut it. Move raw data, edge-case metrics, and rarely-used settings behind drill-downs or secondary views.

**Example:** Hide engine RPM and battery voltage. Surface only: vehicle status, active alerts, driver location.

---

### Law 2: Organize
Group related information so many things feel like fewer things.

**Pattern:** Use consistent clusters:
- `Status` — is everything OK right now?
- `Alerts` — what needs attention?
- `History / Reports` — what happened?

Never scatter the same type of data across multiple tabs or panels.

**Example:** All overdue maintenance items belong in one Alerts section — not split between a vehicle detail page and a calendar.

---

### Law 3: Time
Speed feels like simplicity. Slowness feels like complexity.

**Rules:**
- Critical views (map, alert count) must load in under 1 second
- Use skeleton screens while data loads — never blank states
- Pre-fetch the most-used view (usually the live map)
- Show stale data with a timestamp rather than blocking on fresh data

**Metric:** The user must answer "is anything wrong?" in under 3 seconds.

---

### Law 4: Learn
Reduce the need to learn by using familiar patterns. Teach inline when needed.

**Rules:**
- Use universally understood icons (car pin on map, wrench for maintenance)
- Attach tooltips to real actions, not abstract concepts
- First-time users should understand the dashboard in under 60 seconds without a manual

**Example:** Instead of "HOS violation," show "Driver Jonas has been driving 9h — break required."

---

### Law 5: Differences
Use complexity in a few places to make simplicity elsewhere stand out.

**Rules:**
- The default dashboard state should be calm and minimal
- Reserve red for genuine emergencies — never for decoration
- Use color, size, and weight contrast to create urgency hierarchy

**Alert hierarchy:**
```
🔴 Red   = Critical now     (breakdown, accident, geofence breach)
🟠 Orange = Coming soon      (maintenance due, inspection scheduled)
⚪ Grey   = Informational    (route completed, vehicle returned)
```

---

### Law 6: Context
Peripheral information is not peripheral to the user.

**Rules:**
- Keep secondary context (weather on route, driver contact, fuel price) one click away — not on the main view
- Clicking an alert should navigate directly to the view that resolves it
- Never make the user re-navigate after taking an action

**Example:** Clicking a red "breakdown" alert opens that vehicle's detail + nearest service center — not just the alert log.

---

### Law 7: Emotion
Small emotional details create trust and reduce fatigue.

**Rules:**
- Show a calm "All systems normal ✓" state prominently — not just alerts
- Add subtle positive feedback when tasks complete (job marked done, inspection passed)
- Avoid pure utility aesthetics — they cause disengagement over time

**Example:** When all vehicles return safely, briefly show "Fleet complete — good day 🚢" before resetting.

---

### Law 8: Trust
Honesty is simpler than perfection.

**Rules:**
- Always show data freshness ("Updated 2 min ago")
- Show GPS signal quality per vehicle
- Never display "Live" if data is delayed — show the actual lag
- Surface sync errors clearly rather than hiding them

**Example:** If GPS loses signal, show "Last seen: 14:32" rather than a frozen position.

---

### Law 9: Failure
Some things cannot be made simple — accept it and isolate it.

**Rules:**
- Complex views (compliance reports, IFTA logs, ELD exports) belong in a dedicated Reports section
- Never let regulatory complexity bleed into the main operational view
- Power users get full data access; default users see the simplified version

**Example:** Monthly fuel tax reports are inherently complex — give them their own page, never surface them on the dashboard home.

---

### Law 10: The One Law
> *"Simplicity is about subtracting the obvious and adding the meaningful."*

**For every screen, ask:**
1. What is the user trying to decide or do right now?
2. What is the minimum information they need to do it?
3. What would make them smarter, not just informed?

**Applied to fleet SaaS:**
- Subtract: redundant status labels, decorative charts, columns that restate each other
- Add: predictive maintenance warnings, cost-per-km trends, route efficiency scores

---

## Dashboard Layout Template

Use this as the default structure when building a fleet or operations dashboard:

```
┌─────────────────────────────────────────────────┐
│  TOP BAR (3–4 metrics, 3-second glance)         │
│  [Fleet status] [Alert count] [Fuel] [On route] │
├──────────────┬──────────────────────────────────┤
│  LEFT        │                                  │
│ SIDEBAR      │         MAIN VIEW                │
│              │   (Map or selected vehicle detail)│
│ Scrollable   │                                  │
│ vehicle      │                                  │
│ list         │                                  │
│ with         │                                  │
│ status       │                                  │
│ badges       │                                  │
└──────────────┴──────────────────────────────────┘
```

**Top bar contains:** Overall status message OR alert count, vehicles on route vs total, current fuel cost, next maintenance due.

**Left sidebar contains:** Scrollable list of all vehicles with color-coded status badge, searchable, click to load detail in main view.

**Main view contains:** Live map by default. Switches to vehicle detail or alert resolution view on click.

---

## User Task Frequency Guide

Use this to decide where to place features:

| Frequency | Task Examples | Placement |
|-----------|--------------|-----------|
| Daily (< 3 sec) | Any alerts? All vehicles out? | Top bar |
| Daily (< 10 sec) | Where is vehicle X? | Sidebar + map |
| Weekly (< 30 sec) | Maintenance due soon? Driver hours? | Sidebar alerts / orange badges |
| Monthly (1–2 min) | Fuel costs, efficiency reports | Reports section |

---

## Code Guidelines

When implementing components based on these laws:

- **Reduce:** Never render more than 4 KPI cards in the top bar
- **Organize:** Use consistent component naming: `AlertBadge`, `VehicleListItem`, `StatusBar`
- **Time:** Lazy-load vehicle detail; eagerly load map + alert count
- **Differences:** Define alert colors as design tokens, never inline
- **Trust:** Every data component must accept a `lastUpdated` prop and display it

```javascript
// Example: Alert badge following Law 5 + Law 8
<AlertBadge
  severity="red" // red | orange | grey
  count={3}
  label="Breakdowns"
  lastUpdated="2 min ago"
  onPress={() => navigateTo('alerts/critical')} // Law 6: direct navigation
/>
```

---

## Quick Checklist Before Shipping Any Screen

- [ ] Can the user answer their primary question in under 3 seconds?
- [ ] Is red used only for genuine emergencies?
- [ ] Does clicking an alert take the user directly to the fix?
- [ ] Is data freshness visible?
- [ ] Are complex reports isolated from the operational view?
- [ ] Is the default state calm (not alarming when nothing is wrong)?
- [ ] Have you removed everything that doesn't serve a decision?
