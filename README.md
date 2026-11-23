# Winnow

**What the human wrote, kept, and cut.**

Winnow turns [Open WebUI](https://github.com/open-webui/open-webui) chat exports into a plain-language report on a long human/AI writing project. It helps answer three questions:

- Where did the human's effort go?
- How much of the prose did the human write?
- How much of the AI's output was abandoned?

**Try it:** https://mclellan.github.io/winnow/

The page opens with a clearly labeled sample project. All sample figures are made up.

## Privacy

Your chat logs are read by your browser and are not uploaded. Winnow has no backend, accounts, or analytics.

- Corrections and sorting rules are saved in your browser's local storage. They do not sync to other browsers or devices, and people you share the link with will not see them.
- The page loads D3 from cdnjs and fonts from Google Fonts. Those providers can see a normal web request, but never your chat data.

## Quick start

1. Export your chats from Open WebUI as JSON. You can export a single chat from its menu, or all chats from the data settings. Menu names can vary by version.
2. Open Winnow and click **Add chat logs**, or drag the files onto the page. You can add several files.
3. On **Summary**, click **Review** to step through messages that need a decision.
4. Use **Clean up** for long or repeated messages, working time, and advanced sorting rules.
5. Use **Messages** to inspect the project in **Chapters**, **List**, or **Full map**.

Before exporting, select the branch you consider final in each chat. Winnow treats the selected branch as the kept version.

## Sorting your messages

Every piece of text falls into one of these categories:

| Category | Meaning | Counts toward |
|---|---|---|
| **Prose** | Story text written by the person | Human prose |
| **Instruction** | Direction to the AI | Human direction, never manuscript |
| **AI story** | AI output | Kept or abandoned AI words |
| **Continuity** | Summaries, handoffs, story bibles, and record keeping | Set aside |
| **Repeated** | Text already counted elsewhere | Set aside |

You can correct the sorting at several levels:

- **One passage:** open a message and click the passage to switch its category.
- **Part of a passage:** highlight the text, then choose its category from the small menu.
- **One message:** use **Sort this message** in the details panel.
- **Handoff documents:** under **Advanced sorting**, exclude a word or filename such as `bible.md`.
- **HTML tags:** under **Advanced sorting**, choose how text inside tags such as `<scene>` or `<note>` counts.
- **Built-in markers:** under **Advanced sorting**, turn off any default such as `[square brackets]`, `((double parentheses))`, `OOC:`, or `<details>` if your writing uses it differently.
- **Line labels:** add your own labels such as `Director:`. Each one gets a checkbox beside the built-in line markers.
- **Long messages:** set a word limit, then mark flagged messages as continuity, keep them counted, or ignore them.
- **Technical failures:** use **Ignore** on broken inputs or outputs, such as a model stuck in a loop. Ignored messages are removed from words, attempts, repeat detection, and hours. They appear in their own section and can be restored.

## Reviewing faster

- **Chapters** shows one row per chat. Bars above a row show abandoned attempts, and alerts stay visible at any zoom. Click a row to see its messages.
- **List** shows every message in a keyboard-friendly list with the same filters.
- **Full map** keeps the complete branching tree.
- **Suggestions** flag likely loops, reused AI outputs, long messages, and unsure sorting. Each flag states its reason.
- **Batch preview** lets you uncheck false positives before applying a suggestion.
- **History** records your decisions, and **Undo** reverses the latest one.

## How the numbers work

- **Kept** is AI story text and human prose on the branch selected at export.
- **Abandoned** is AI story text and human prose on every other branch.
- **Repeated text** is a run of 40 or more identical words from one earlier message. Capitalization and punctuation are ignored.
- **Retried prompts** count only changed words when at least 60% of their words match an earlier version in order. The kept version receives credit for shared words.
- **Attempts rejected** counts extra branches at each fork.
- **Hours worked** adds the time between consecutive messages and treats a pause over the break setting (20 minutes by default) as time away.
- **Reading time** assumes 238 words per minute.

## Limitations

Treat the results as a careful estimate. They are not proof of authorship.

- **Open WebUI only.** Other chat export formats are not supported.
- **Kept is an upper bound.** Winnow cannot see your final manuscript. Kept AI text can include planning, outlines, or feedback that never reached the book.
- **Work outside the chat is invisible.** Editing in another app, planning, research, and rereading are not counted, so human effort may be understated.
- **Hours are inferred.** Timestamps do not show whether someone was actively working. AI generation time is removed only when the export records it, and the result changes with the break setting.
- **Prose sorting is a heuristic.** Unmarked text is judged by its wording and will sometimes be wrong. Review flagged messages before presenting results.
- **Repeat detection is literal.** Reworded summaries are not detected, and passages under 40 identical words are not treated as copies.
- **Retry detection can over-merge.** Stock instructions reused within a few turns count once. Prompts under 4 words are always counted in full.
- **Pasted text can look human.** Text from outside the loaded logs, including AI output from another tool, can be classified as human prose.
- **Reused AI outputs need a decision.** Winnow flags them, but you decide whether each one is continuity or story.
- **Word counts are approximate.** Hidden reasoning and tool blocks are excluded.
- **Saved corrections are local.** Clearing browser data removes them.

## Sharing your results

Click **Share** after loading your chat logs. Winnow creates a read-only link and can also download a PNG summary card.

- **Included:** word and effort totals, hours per week without dates, node shapes, categories, kept or abandoned status, and rough message lengths.
- **Never included:** message text, chat titles, filenames, exact timestamps, model names, or message IDs.
- **No upload:** the data is compressed into the part of the URL after `#`, which browsers do not send to GitHub. Anyone with the link can read what it contains, and links cannot be revoked.
- **Long projects:** if a full link would exceed 8,000 characters, Winnow turns off the node map. You can turn it back on, but some apps may cut off long links.
- **Self-reported:** a shared link can be edited by someone who understands the format. Shared dashboards are labeled as not independently verified.

## Run it locally

Winnow is a single static page with no build step. Open `index.html` in a browser, or serve the folder:

```bash
python -m http.server 8000
```

Then visit http://localhost:8000. An internet connection is needed for D3 and web fonts. Without D3, the map displays but cannot be panned or zoomed.

## Deployment

The site is served by GitHub Pages from the root of the `main` branch. Pushing to `main` publishes a new version.

## License

[MIT](LICENSE)
