# BizPredict AI Android v3.1

Native Android application written in Java. This is the Android adaptation of the Java Swing BizPredict AI desktop project.

## Features
- Android login and registration
- Per-user local SQLite database
- PBKDF2-HMAC-SHA256 password hashing with a random salt
- Add, edit and delete business records
- Dashboard totals and a custom sales/profit chart
- Linear Regression and a small bagged regression-tree (Random Forest-style) model
- Chronological holdout MAE comparison
- PDF report export using Android's document picker

## Requirements
- Android Studio (recommended current stable)
- JDK 17 (Android Studio bundled JDK is suitable)
- Android SDK Platform 35
- Android phone with Android 6.0 (API 23) or newer, or an emulator

## Open and run
1. Download and extract `BizPredictAI-Android.zip`.
2. Open Android Studio.
3. Choose **Open** and select the extracted `BizPredictAI-Android` folder (the folder containing `settings.gradle`).
4. Allow Gradle sync and install any requested Android SDK components.
5. Connect your Android phone with USB debugging enabled, or start an emulator.
6. Press **Run** and select the phone/emulator.

No MySQL server, internet connection, Weka dependency, or external PDF library is required at runtime. User accounts and records are stored locally on the device.

## First use
1. Tap Create account.
2. Enter a username (3-60 letters, numbers, dot, underscore or hyphen) and password (at least 8 characters).
3. Log in.
4. Add monthly business data. Use `YYYY-MM` format, e.g. `2026-01`.
5. Add at least 8 monthly records for the same business name to enable model comparison.
6. Open Predict, enter expected sales, expenses and other costs.
7. Compare Linear Regression and the bagged regression-tree ensemble.
8. Tap PDF and choose where to save the report.

## Prediction methodology
- Historical target: profit = sales - expenses - other costs.
- Input features: month index, sales, expenses and other costs.
- Linear Regression is fitted with a regularized normal-equation solver.
- The tree ensemble bootstraps historical records and averages 100 regression trees.
- The last chronological 20% of records is used as a basic holdout set to calculate MAE; both models are then retrained on all available history.
- The model with the lower holdout MAE is selected for the estimate.

## Important limitations
- This is an educational prototype, not a production finance application.
- SQLite data is stored on the phone. It does not automatically sync with the desktop app or across phones.
- The Random Forest implementation is a compact educational bagged regression-tree ensemble, not Weka's RandomForest implementation.
- With only eight records, validation scores are unstable. Use substantially more monthly observations and rolling time-based validation for a serious evaluation.
- A model can estimate profit from supplied future sales/cost inputs; it cannot know future sales automatically.
- Protect the phone with a screen lock and keep backups. Uninstalling the app or clearing its data can remove locally stored records.
