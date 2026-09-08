# Noosphere Polymath OS

![Status: UI prototype](https://img.shields.io/badge/status-UI%20prototype-amber)
![Runtime: browser](https://img.shields.io/badge/runtime-browser-blue)
![Chart.js: 4.4.0](https://img.shields.io/badge/Chart.js-4.4.0-ff6384)

A browser-based creative operations dashboard prototype, built with HTML, Tailwind CSS, and Chart.js to explore campaign metrics, project progress, and insight presentation.

 It brings performance trends, channel allocation, production activity, and project status into one screen—a concrete starting point for reviewing a creative workspace before committing to a data model or service architecture.

> [!IMPORTANT]
> This is a front-end demonstration, not a production application or an operating system. All metrics, project records, scores, and insights are hard-coded examples. The “AI” and “Live” labels describe the interface concept; there is no model inference, analytics integration, or real-time data connection.

[Quick start](#quick-start) · [Capabilities](#capabilities) · [Architecture](#architecture) · [Development](#development) · [Roadmap](#roadmap) · [Contributing](#contributing)

## Capabilities

| Area | Implemented behavior |
| --- | --- |
| Dashboard summary | Four sample KPI cards with inline SVG sparklines: creative output, brand reach, engagement, and campaign ROI. |
| Performance charts | Five Chart.js canvases: a 12-week line chart, score doughnut, creative-dimensions radar, channel-allocation doughnut, and weekly-production bar chart. |
| Project overview | Four static project rows with progress bars, asset counts, and status labels. |
| Insight presentation | Five predefined messages cycle through a JavaScript typewriter animation. |
| Responsive layout | Breakpoint-based grids, a desktop sidebar, and a mobile drawer toggled by the menu button and dismissed through its overlay. |
| Visual styling | Dark backgrounds, translucent panels, gradients, hover states, and entrance animations. |

### Scope and limitations

- **Navigation is presentational.** Sidebar links point to `#`; they do not load separate views.
- **Action controls are placeholders.** Search, notifications, Export, New Project, and View all have no application handlers.
- **State is transient.** Menu and animation state live in memory. There is no database, browser-storage persistence, authentication, or application API.
- **External assets are required.** Styling, charts, icons, and fonts depend on third-party services. The page is not self-contained for offline use.
- **Engineering checks are not automated.** The repository has no test suite, lint configuration, CI/CD workflow, or documented performance and accessibility audit.

## Quick start

### Prerequisites

- A modern browser with JavaScript enabled.
- Internet access to load the external dependencies listed below.
- Git to clone the repository; Python 3 for the local-server example.

No Node.js installation, package installation, API keys, or build step is required.

```sh
git clone https://github.com/zazieproductions/Noosphere-Polymath-OS.git
cd Noosphere-Polymath-OS
python3 -m http.server 8000 --bind 0.0.0.0
```

Open **http://localhost:8000** when the browser and server run on the same machine. In a hosted development environment, open its forwarded port URL instead. Stop the server with `Ctrl+C`.

> [!NOTE]
> Binding to `0.0.0.0` supports forwarded previews but also exposes the server on available network interfaces. For local-only access, use `--bind 127.0.0.1`. Python's development server is not intended for production hosting.

You can also open `index.html` directly in a browser; its remote dependencies still require network access. A local HTTP server is preferable when reviewing deployment behavior.

### Explore the interface

1. Review the KPI cards and hover over chart data to inspect tooltips. The score gauge intentionally has tooltips disabled.
2. Resize the browser below the desktop navigation breakpoint and use the menu button to open the drawer. Click the overlay to close it.
3. Watch the **Muse AI Insights** panel type, pause, erase, and advance through the predefined messages.
4. Refresh the page to restart the demonstration. No changes are saved.

## Architecture

The application is a single static document. HTML defines the interface and sample records; inline CSS supplies custom visual styles; one inline script binds the mobile menu, initializes charts, and runs animations.

```text
Static HTTP host / local file
└── index.html
    ├── HTML: layout, KPI values, project records, labels
    ├── CSS: custom styles and animations
    └── JavaScript
        ├── Mobile drawer event handlers
        ├── Chart.js defaults and five chart instances
        └── Insight rotation and entrance animation setup

Browser fetches external scripts, stylesheets, and fonts
└── Tailwind CSS · Chart.js · Font Awesome · Google Fonts
```

There is no server-side application, router, module system, or client framework. The closing script executes after the dashboard markup and addresses elements by their IDs. Chart.js is expected to be available as the global `Chart` object.

### Design trade-offs

The single-file structure keeps setup short and makes the complete rendering path easy to inspect. Static delivery does not require an application server or database, so the document can be served by a conventional static host.

That simplicity is appropriate for a UI prototype, not evidence of production readiness. Data and presentation are coupled, desktop and mobile navigation markup is duplicated, and initialization has no dependency-failure recovery. Larger datasets, additional screens, or live integrations would benefit from explicit boundaries between data access, rendering, and interaction logic. There are no benchmarks or load-test results in this repository.

### Runtime dependencies

| Dependency | Source / version in `index.html` | Role |
| --- | --- | --- |
| Tailwind CSS | `cdn.tailwindcss.com` — unpinned | Browser-generated utility styling. |
| Chart.js | jsDelivr — `4.4.0`, UMD build | Canvas chart rendering and interactions. |
| Font Awesome | cdnjs — `6.5.1` | Interface icons. |
| Google Fonts | Google Fonts CSS API | Inter and Space Grotesk typefaces. |

Dependencies are referenced directly in the document's `<head>`; there is no package manifest or lockfile. Versioned URLs for Chart.js and Font Awesome constrain those requests, but the external dependency set is not fully pinned or vendored.

## Configuration and customization

There are no environment variables, configuration files, or runtime settings. Edit [`index.html`](index.html) and refresh the browser.

| Change | Location |
| --- | --- |
| Page title and branding | `<title>` and the `HEADER` markup. |
| KPI values and sparklines | `KPI CARDS` markup and inline SVG paths. |
| Project names, statuses, and progress | `Creative Projects` markup; update both progress widths and displayed percentages. |
| Chart labels, values, and appearance | `TREND CHART`, `AI SCORE GAUGE`, `RADAR CHART`, `DONUT CHART`, and `BAR CHART` script sections. |
| Rotating insight text and timing | The `insights` array and `typeEffect()` function under `TYPEWRITER EFFECT`. |
| Theme and layout | The `<style>` block, Tailwind classes, and `CHART.JS THEME` defaults. |
| Navigation labels | Both desktop and mobile sidebar markup. |

For example, replace the labels and values in the bar chart's existing `data` object:

```js
data: {
    labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
    datasets: [{
        label: 'Assets Created',
        data: [10, 14, 18, 22, 16, 6, 4],
        // Retain the existing dataset styling here.
    }]
}
```

Keep label and data-array lengths aligned. Values repeated in markup and JavaScript are not synchronized: changing the score doughnut, for example, does not change its center label. These edits change the demonstration, not an underlying analytics calculation.

## Development

### Project structure

```text
.
├── README.md   # Scope, setup, architecture, and contributor guidance
└── index.html  # Complete dashboard: markup, styles, sample data, and scripts
```

### Workflow

1. Start the local server from the repository root.
2. Make a focused change in `index.html`, preserving the existing section organization.
3. Refresh the browser and inspect both the console and network panel.
4. Check the affected behavior at narrow and wide viewport sizes.
5. Review `git diff --check` and `git diff` before submitting a pull request.

There is no hot-reload process or build output. Source edits are served directly after refresh.

### Manual verification

Until automated checks are introduced, use this checklist and report the browser and viewport sizes tested:

- [ ] The page loads with all five charts and no JavaScript errors under normal network conditions.
- [ ] Chart tooltips and legends behave as configured; the score gauge remains non-interactive.
- [ ] The mobile menu opens and the overlay closes it; desktop navigation remains visible at wide widths.
- [ ] Insight text completes a typing/deletion cycle and advances to the next message.
- [ ] Cards, labels, and project rows remain readable without unintended horizontal overflow.
- [ ] Keyboard focus, icon-button labeling, chart alternatives, and motion preferences have been reviewed for the changed UI. Record limitations rather than assuming accessibility compliance.
- [ ] Blocking an external dependency is tested when changing loading behavior; any resulting degradation is documented.

## Deployment, reliability, and security

For a **demonstration deployment**, serve `index.html` from a static host over HTTPS. No server-side runtime or build artifact is needed. Hosting configuration, security headers, and deployment automation are not included.

Before using this as the basis for a production application:

- **Control asset delivery.** Replace the Tailwind browser CDN with generated CSS; pin dependencies and consider self-hosting scripts, styles, and fonts. Review dependency licenses before redistribution.
- **Handle dependency failures.** If Chart.js fails to load, the first `Chart` reference throws and subsequent chart and insight initialization does not run. Add explicit failure handling and useful fallback content.
- **Define a content security policy.** Scripts and styles are currently inline, and remote resources have no Subresource Integrity attributes. External scripts execute in the page context; evaluate approved origins, integrity checks, and a nonce/hash or external-file strategy before enforcing CSP.
- **Keep secrets out of the client.** The current code has no application-data requests, but the browser contacts third-party asset providers. Do not embed credentials or sensitive records in a publicly served document. Any future data integration needs its own access-control and privacy design.
- **Measure rendering cost.** Runtime CSS generation, chart rendering, backdrop filters, and recurring animations warrant profiling on target devices. No performance budget or browser-support matrix is established.
- **Complete accessibility work.** Add accessible names for icon-only controls, chart text alternatives, reduced-motion behavior, and appropriate drawer focus management. These are not fully implemented today.

## Roadmap

The following are **proposed next steps**, not committed milestones or existing features:

- [ ] Establish baseline HTML/JavaScript checks and browser smoke tests in CI.
- [ ] Address accessibility gaps and document a tested browser/viewport matrix.
- [ ] Adopt reproducible asset delivery with generated CSS and pinned dependencies.
- [ ] Separate sample data, presentation, and interaction logic as the interface grows.
- [ ] Define and implement navigation and action-control behavior, including loading, empty, and error states where needed.
- [ ] Specify data contracts, authentication, and persistence requirements before connecting real services.

## Contributing

Keep changes small, reviewable, and consistent with the prototype's documented scope. For a substantial architectural change, explain the problem and proposed approach before introducing a framework, backend, or build pipeline.

A pull request should include:

- The problem addressed and a concise description of the change.
- Screenshots at desktop and mobile widths for visual changes.
- Manual verification results, including browser details and known limitations.
- Updated documentation for changed behavior, dependencies, or setup steps.

Do not present placeholder UI as a working integration, commit credentials, or introduce dependencies without explaining their purpose and delivery implications. There is no separate contribution policy or automated merge gate in the repository today.

## License

No `LICENSE` file is currently included. Do not assume an open-source license or permission to use, modify, or redistribute the project beyond rights otherwise granted. Ask the repository owner to clarify licensing before reuse. Third-party libraries and fonts remain subject to their respective licenses.
