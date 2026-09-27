# Persian Schedule Builder

A mobile-first, right-to-left schedule builder for turning a class timetable into an editable weekly schedule. It runs entirely in the browser and can be published as a static GitHub Pages site.

**Live app:** [Open the schedule builder](https://samansbn.github.io/persian-schedule-app/)  
**Code walkthrough:** [Read the section-by-section guide](https://samansbn.github.io/persian-schedule-app/code-guide.html)

## Features

- Import an HTML timetable file or paste table HTML/plain text.
- Detect common Persian timetable columns and normalize weekdays and Persian/Arabic digits.
- Review and edit extracted classes before creating the schedule.
- Add or delete classes manually; set weekdays, class times, location, instructor, exam date/time, and lab-exam status.
- Choose one of four schedule appearances, with bilingual names: **Nightfall · شبانه**, **Moonlit · شبانه روشن**, **Sandstone Cards · کارت روشن**, and **Violet Cards · کارت بنفش**.
- Download a standalone HTML schedule or an opaque JPEG image (`schedule.jpeg`). The image is rendered at 3× scale and 98% JPEG quality.
- View the app in a responsive, mobile-first RTL layout. The standalone schedule HTML keeps a fixed 420px design canvas that phones scale proportionally, preserving its row layout.

## Quick start

1. Open `index.html` in a current browser.
2. Choose **انتخاب فایل HTML** to import an `.html`/`.htm` timetable, or paste the source/text into the input area.
3. Select **استخراج خودکار**. The app opens the review step with the detected classes.
4. Edit any fields, add/delete rows as needed, and select **تولید برنامه**.
5. Pick a schedule style. The selection updates the preview and is included in the standalone HTML download.
6. Choose **ذخیره به‌صورت JPEG** for an image, or **دانلود فایل HTML** for a standalone schedule page.

You can also skip extraction and choose **حالت دستی** to start with an editable blank class.

## Input formats

### HTML timetable

The parser finds a table and maps columns by Persian header names. It recognizes course, professor, weekday, class time, exam date, and location columns. Column order can vary, and unrelated extra columns are ignored. Header matching accepts common variants such as `استاد`/`اساتید` and `تاریخ امتحان`/`امتحان`.

### Pasted text

The fallback parser scans copied text for weekday anchors and nearby time/date values. It is intended for common copy/paste layouts where values are separated by lines or tabs. Because text-only input has less structure than a table, review the extracted rows before building the schedule.

## Privacy and external resources

The app has no backend. File reading, parsing, editing, and schedule generation run in the browser; timetable contents are not uploaded by this app. The page loads the Vazirmatn font from Google Fonts and `html2canvas` 1.4.1 from cdnjs. Those services receive their normal asset requests. JPEG export requires `html2canvas` to have loaded; importing, editing, previewing, and HTML export do not depend on it.

The app-theme preference is stored locally in `localStorage` under `scheduleAppTheme`. It does not store timetable contents.

## Code walkthrough

Open the **`</>`** icon in the app's top-right corner to read the [section-by-section code walkthrough](https://samansbn.github.io/persian-schedule-app/code-guide.html). It explains the document structure, both CSS systems, theme handling, parsing routes, editable state, schedule rendering, and HTML/JPEG exports. Formatted excerpts are followed by an expandable, escaped copy of the complete deployed `index.html` source. The guide is a separate static page and does not execute the displayed source.

The style dropdown keeps stable internal IDs (`night`, `night-light`, `light-cards`, `purple-cards`) while displaying these readable bilingual names:

| ID | English name | Persian name |
| --- | --- | --- |
| `night` | Nightfall | شبانه |
| `night-light` | Moonlit | شبانه روشن |
| `light-cards` | Sandstone Cards | کارت روشن |
| `purple-cards` | Violet Cards | کارت بنفش |

## Repository layout

```text
.
├── index.html                 # The complete browser app and deployed home page
├── code-guide.html            # Responsive guide with formatted excerpts and full source listing
├── README.md                  # Project, usage, architecture, and deployment notes
└── .github/workflows/
    └── pages.yml              # Publishes the static site to GitHub Pages
```

The app is intentionally delivered as a single HTML file: markup, styles, and application logic are together in `index.html`, with external font and image-rendering assets loaded at runtime.

## GitHub Pages deployment

The repository publishes through GitHub Actions. The workflow runs on every push to `main` and can also be started manually from the Actions tab.

For a new repository:

1. Open **Settings → Pages**.
2. Set the build and deployment source to **GitHub Actions**.
3. Push to `main` or run **Deploy GitHub Pages** manually.
4. Wait for the `github-pages` environment deployment to finish. The app is served from `index.html`; `code-guide.html` is the separate guide page.

GitHub Pages requires a public repository on GitHub Free. A custom workflow packages the repository root and deploys it as a static artifact. See [GitHub's custom Pages workflow documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages).

## Browser support and limitations

- Use a modern browser with JavaScript enabled.
- HTML table parsing uses the browser's `DOMParser`.
- The plain-text parser uses heuristics; inspect the review step for ambiguous or irregular source text.
- JPEG export depends on the CDN-hosted `html2canvas` script and browser canvas support.
- The standalone schedule preserves the selected appearance and schedule data, but it remains a client-generated HTML file.

## Maintenance notes

- Add supported column-heading aliases in `HEADER_MAP` in `index.html`.
- Adjust recognized weekday names in `DAY_ORDER`, `DAY_COMPACT`, `isDayLine`, and `normalizeDay` together.
- Keep the export row markup in `rowHtml()` shared by the live preview and downloaded HTML.
- Add any new schedule theme to the style dropdown, `.sched-page[data-style="…"]` CSS, and `imageBackgroundForStyle()` so preview, HTML, and JPEG output stay aligned.
- Keep the GitHub Pages workflow's minimum permissions scoped to reading repository contents and deploying Pages artifacts.

## License

No license is currently declared. Ask the repository owner before reusing or redistributing this project.
