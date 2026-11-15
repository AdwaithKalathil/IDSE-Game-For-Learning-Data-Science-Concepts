# Overfitting Arena

A browser-based mini-game for an **Introduction to Data Science** course. The player fits polynomial regression models to two randomly generated noisy datasets and tries to choose the model complexity that performs best on unseen observations.

## Learning objectives

By the end of the game, a learner should be able to:

1. **Recognize underfitting:** explain why a model that is too simple can miss the structure in a dataset and have high training and test error.
2. **Recognize overfitting:** identify the pattern where training error continues to decrease while held-out test error increases because a flexible model fits noise or sample-specific details.
3. **Use generalization to select model complexity:** explain why the best model is not necessarily the one with the lowest training error, and why performance on held-out data is a better guide.

## How to play

1. Open `index.html` in a modern browser.
2. In Round 1, move the **Polynomial degree** slider from 1 to 10. Observe how the orange fitted curve changes and how training MSE changes.
3. While exploring, the held-out test error and test-error curve remain hidden. Choose the degree you believe will perform best on unseen data, then click **Lock degree … as my guess**.
4. The game reveals the test MSE values, the degree with the lowest test MSE, and your score. The score is based on your guessed degree's test error relative to the best test error—not its training error.
5. After the reveal, move the slider again to compare training MSE, test MSE, and score live.
6. Click **Start Round 2**. The game generates a new dataset using a different underlying signal and a newly sampled noise level. Make a fresh prediction.
7. Read the **Learning debrief** after Round 2. Click **Play again** to generate two new datasets.

## Exporting playtest sessions

After completing Round 2, enter the participant's name or an anonymous ID in the **Player name / ID** field and click **Export session data**. The browser downloads a JSON file named `playtest_<player>_<timestamp>.json`. It contains the player identifier, export timestamp, and one record per round with the guessed degree, best test degree, score, training/test MSE for the guess, and the signal name and description.

Move each downloaded JSON file into `playtest_evidence/` to keep the raw session logs with the project. The browser downloads the file to its normal download location; a client-side page cannot silently save it directly into the project folder. Prefer anonymous participant IDs unless names are necessary for your course's evidence requirements.

## Concept: underfitting, overfitting, and generalization

Polynomial regression represents a prediction as

\[
\hat y(x) = a_0 + a_1x + a_2x^2 + \cdots + a_dx^d,
\]

where `d` is the polynomial degree. A low degree restricts the shapes the model can represent. This can cause **underfitting**: the model is too simple to capture the pattern in the data, so both training and test error are often high.

As degree increases, the model becomes more flexible. It can usually fit the training observations more closely, so training error tends to decrease. At sufficiently high complexity, the fitted curve can start following random fluctuations in the training sample. This is **overfitting**. Those fluctuations are not reliable patterns, so predictions on new observations can become worse even while training error continues to improve.

The game uses mean squared error (MSE):

\[
\mathrm{MSE} = \frac{1}{n}\sum_{i=1}^{n}(y_i - \hat y_i)^2.
\]

For each round, the program creates separate training and test samples from the same randomly chosen underlying signal and noise level. The polynomial coefficients are fitted using **only the training sample**. The test sample is held out and used only to evaluate each fitted degree. The program fits every degree from 1 through 10, calculates both MSE values, and finds the lowest test-MSE degree from the generated test data. No degree is hard-coded as the winner.

The player first sees the training error and fitted curve, then commits to a degree before the test results are unsealed. After the reveal, the player can change degree and compare both errors. This sequence makes the difference between fitting the observed sample and generalizing to new data part of the game mechanic.

### Important interpretation note

The test set is finite and randomly sampled, so the best test degree can vary between plays. The measured test MSE also includes the randomly generated observation noise. The game illustrates the bias/variance and generalization trade-off; it is not a claim that one particular polynomial degree is universally optimal.

For teaching purposes, the game uses this held-out set to compare candidate degrees and calculate the score. In a real machine-learning workflow, model selection should be done on a validation set (or with cross-validation), while a separate final test set should be reserved for one final, unbiased evaluation. Repeatedly choosing models based on the same test set makes that set part of the tuning process.

## Implementation notes

- **Single-file app:** all HTML, CSS, and JavaScript are in `index.html`.
- **No external dependencies:** charts are drawn with the Canvas API; there are no libraries, build steps, or network requests.
- **Live data generation:** each round samples new `x` values and Gaussian noise using `Math.random()`.
- **Live model fitting:** polynomial least squares is calculated in JavaScript for degrees 1–10. The implementation uses Householder QR least squares rather than directly solving the normal equations, which is more numerically stable for the polynomial design matrix.
- **Held-out evaluation:** test observations are generated independently and are not used to estimate polynomial coefficients.
- **Local execution:** all computation runs in the browser. No player data is transmitted.

## Playtest evidence (complete after testing with classmates)

> Do not submit this section as completed evidence until peers have actually tried the game. Replace the placeholders below with real observations and feedback.

| Participant (name or anonymous ID) | What they tried / degree guessed | Outcome or observation | Feedback / confusion noticed |
|---|---|---|---|
| [Peer 1] | [ ] | [ ] | [ ] |
| [Peer 2] | [ ] | [ ] | [ ] |
| [Peer 3] | [ ] | [ ] | [ ] |

### Changes made after playtesting

- [Record any change made in response to real playtest feedback.]

### Reflection

- [Did players identify underfitting and overfitting without being told the answer?]
- [Could players explain why the best training fit was not always the best test performer?]
- [What would you improve in another iteration?]
