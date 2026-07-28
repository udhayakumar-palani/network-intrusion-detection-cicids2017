# Live demo cheat-sheet — Colab walkthrough

**For:** 10-minute presentation, demo segment on Slide 10
**Time budget:** 90 seconds total in Colab
**Critical:** practise this once before recording

---

## Pre-recording setup (do 30 minutes before)

1. **Open Colab** in a separate browser tab/window
2. **Open the notebook** `IDS_CICIDS2017_Notebook.ipynb`
3. **Run every cell EXCEPT the SHAP cell** — Runtime → Run all, then click Stop just before the SHAP cell
4. **Verify the runtime is warm** — top right should say "Connected" with a green tick
5. **Position windows** — slides on monitor 1, Colab on monitor 2 (or alt-tab if single screen)
6. **Test alt-tab** once before going live
7. **Mute notifications** — Slack, email, phone

---

## The 90-second demo flow

### Step 1 — CFS function and selected features (10 seconds)

**Action:** Scroll to Section 4 of the notebook (the CFS code cell)

**Say:**
> "This is the CFS function — two passes, class relevance then redundancy elimination. The output below shows the selected features. Notice the top: Bwd Packet Length Std, Max, Mean — the same features Figure 2 highlighted on the previous slide."

**Where to point:** the printed list under the cell (the "Selected features" output)

---

### Step 2 — CNN-LSTM model summary (10 seconds)

**Action:** Scroll down to Section 8 (CNN-LSTM model definition cell)

**Say:**
> "Here's the proposed CNN-LSTM model summary. Two Conv1D blocks, an LSTM layer, two dense layers. About twenty-five thousand trainable parameters."

**Where to point:** the `Total params` line at the bottom of the model summary output

---

### Step 3 — Run the SHAP cell live (30 seconds)

**Action:** Scroll to Section 10. Click the SHAP cell that contains:

```python
explainer = shap.TreeExplainer(rf)
shap_values = explainer.shap_values(X_shap)
```

**Click the play button (or Shift+Enter)** to execute.

**Say while it runs:**
> "And here's SHAP running live. This computes Shapley values for one thousand test flows in about twenty seconds — TreeExplainer is exact and fast on Random Forest."

**If it takes longer than expected:** keep speaking, don't fall silent. Add:
> "While that runs, remember this is computing exact Shapley values, not approximations — it's the canonical method for tree-based models."

---

### Step 4 — Walk through the SHAP plot (30 seconds)

**Action:** Scroll to the SHAP global importance plot output (the bar chart showing top features)

**Say:**
> "And here's the result — the global feature importance plot. Notice the top features: backward packet length statistics. These are the same features that Pearson correlation ranked first on Slide 6 — two completely independent feature-importance methods agreeing on the same answer. That's exactly the kind of audit trail an analyst needs to trust an alert."

**Then alt-tab back to slides** and say:
> "Switching back to slides."

---

## Failure recovery — if something breaks

### If Colab disconnects mid-demo:
**Say:** "Looks like Colab has timed out — let me show you the pre-computed result from earlier."
**Action:** Click on the cell that already has cached output below it — the SHAP plot will still be visible from the previous run. Continue with Step 4.

### If the SHAP cell errors out:
**Say:** "Looks like there's a small library hiccup — let me just show you the pre-computed plot."
**Action:** Scroll to the saved output cell underneath, point to the bar chart, continue with Step 4.

### If you go over time:
**Skip Step 4's full walkthrough** and just say:
> "I'll point you to Figures 5 and 6 in the report for the full SHAP analysis."
**Action:** alt-tab back to slides immediately.

---

## What NOT to do during the demo

- **Don't retrain anything.** No `model.fit()` calls. Ever.
- **Don't open new files or notebooks.** Stay in the one you pre-loaded.
- **Don't apologise** if something looks slightly off. Move on.
- **Don't read the code aloud.** The audience can't follow it; describe what it does instead.
- **Don't zoom the Colab UI** mid-demo. Set the zoom level beforehand.

---

## Mental checklist immediately before going live

- [ ] Slides open and on slide 10
- [ ] Colab open in second tab/window, runtime connected
- [ ] All cells pre-run except the SHAP cell
- [ ] Notifications muted
- [ ] Microphone tested
- [ ] Recording started
- [ ] Water nearby

---

## Timing self-check during talk

| Time mark | Should be on slide |
|-----------|-------------------|
| 0:30 | End of Slide 1 (title) |
| 1:20 | End of Slide 2 (motivation) |
| 2:05 | End of Slide 3 (RQs) |
| 3:20 | End of Slide 4 (architecture) |
| 4:05 | End of Slide 5 (dataset) |
| 5:00 | End of Slide 6 (CFS) |
| 5:50 | End of Slide 7 (4 models) |
| 7:05 | End of Slide 8 (results table) |
| 7:55 | End of Slide 9 (ROC/CMs) |
| 9:25 | End of Slide 10 (DEMO) ← check here |
| 10:00 | End of Slide 11 (conclusions) |

If you're behind at the 5-minute mark, skip ahead on Slide 6 (don't read every word of the CFS algorithm box — say "two passes, class relevance then redundancy" and move on). If you're behind at 7 minutes, trim Slide 9's confusion-matrix walkthrough to just one sentence.
