# Grandma’s Bakeria submission site

A single-page Socratica × Ramp Buildathon submission site. It uses plain HTML, CSS, and JavaScript. GitHub Pages can host it because submissions are posted directly to a Google Form.

## Files

- `index.html` — page and submission fields
- `styles.css` — responsive visual design
- `script.js` — validation, submission, and UI states
- `config.js` — the only file you must edit to connect your Google Form

The included `config.js` is connected to the published buildathon Google Form. If you copy this site for another event, replace the form URL and entry IDs as described below.

## 1. Prepare the Google Form

Create or use a form with **short answer** or **paragraph** questions for the fields below. The “Where did you help?” question can be a dropdown, but its choices must match the site’s choices *exactly* (`Inventory`, `Purchasing`, `Marketing`, `Customers`, `Operations`, `Finances`, `Something else`). Do not use a Google Form file-upload question; anonymous HTML posts cannot upload a file.

| Site field | Suggested Google Form question | Recommended question type |
| --- | --- | --- |
| `teamName` | Team name | Short answer |
| `teamMembers` | Team members | Short answer |
| `contactEmail` | Contact email | Short answer |
| `projectName` | Project name | Short answer |
| `codeUrl` | Code link | Short answer |
| `demoUrl` | Demo link | Short answer, optional |
| `problemArea` | Where did you help? | Dropdown or short answer |
| `problem` | What problem did you notice? | Paragraph |
| `solution` | What did you build? | Paragraph |
| `impact` | How does it save time or money? | Paragraph |
| `estimate` | How did you estimate that? | Paragraph, optional |

Keep Google Form required settings aligned with the asterisks on the site. Avoid “limit to 1 response,” sign-in requirements, quizzes, and extra required questions; those settings can make a hidden-iframe submission fail without a readable error. If Google Form has “Collect email addresses” enabled, use a normal contact-email question instead or test the behavior carefully.

## 2. Find the Form URL and entry IDs

1. Open the Google Form’s **published respondent view** and use its full URL, which looks like `https://docs.google.com/forms/d/e/FORM_ID/viewform` (some forms use `/d/FORM_ID/viewform`).
2. Change the final `/viewform` to `/formResponse`. Put that URL into `formResponseUrl` in `config.js`.
3. In the Form editor, choose **More (⋮) → Pre-fill form**. Type a unique marker into each question, such as `TEAM_MARKER`, `PROJECT_MARKER`, and so on. Click **Get link**.
4. Paste the resulting link into a text editor. Each question appears as a pair such as `entry.123456789=TEAM_MARKER`. Match each marker to its `entry.#########` ID, and paste those IDs into the corresponding keys in `config.js`.
5. Check that all 11 IDs are filled in, including optional questions. The site posts optional values as empty strings when left blank.

Example (use your real IDs, not these examples):

```js
window.BAKERIA_FORM_CONFIG = {
  formResponseUrl: "https://docs.google.com/forms/d/e/YOUR_FORM_ID/formResponse",
  entries: {
    teamName: "entry.111111111",
    teamMembers: "entry.222222222",
    contactEmail: "entry.333333333",
    projectName: "entry.444444444",
    codeUrl: "entry.555555555",
    problemArea: "entry.666666666",
    problem: "entry.777777777",
    solution: "entry.888888888",
    impact: "entry.999999999",
    estimate: "entry.101010101",
    demoUrl: "entry.121212121"
  }
};
```

## 3. Test a real submission

After editing `config.js`, submit a test project from the site and **verify a new row appears in the Google Form’s Responses tab or linked Google Sheet**. Check every column. Delete the test response when you’re done. Repeat this test after changing the Form, especially question types, response restrictions, or required settings.

The site uses a native HTML POST aimed at a hidden iframe. Browsers cannot read Google’s response inside that iframe because it is on another origin. The success screen therefore means the form page finished loading after the post; **only the Google Form’s Responses tab can confirm acceptance**. A slow or blocked load shows an error asking for manual verification before retrying, to avoid duplicate submissions.

## 4. Deploy on GitHub Pages

1. Create a GitHub repository and put the five site files at its root. (If you upload this folder as a folder, set the Pages source accordingly or move the contents to the repository root.)
2. Commit and push the files.
3. In the repository, open **Settings → Pages**. Under **Build and deployment**, select **Deploy from a branch**, choose your branch (usually `main`) and `/ (root)`, then save.
4. Open the URL GitHub provides, commonly `https://YOUR_USERNAME.github.io/YOUR_REPOSITORY/`.
5. Send one more test submission from the deployed URL and verify it in Google Forms.

All assets use relative paths, so the site works at a GitHub Pages repository subpath. Google Fonts are optional visual enhancements; system fallbacks render if they do not load.

## Editing the copy or fields

Text and field labels live in `index.html`. If you add, remove, or rename a field, update its matching key in `config.js` and the mapping table here. `script.js` builds the Google Form post from the fields in the page, so every field needs a matching `entry.#########` mapping.
