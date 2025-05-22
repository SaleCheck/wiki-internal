# Firebase Emulator: Local Development Guide

This document provides guidance for using the Firebase Emulator Suite in local development. It includes steps for importing data from production, working with Firestore, and running the emulator reliably.

---

## Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [Importing Firestore Data from Production](#importing-firestore-data-from-production)
4. [Running the Firebase Emulator](#running-the-firebase-emulator)
5. [Additional Notes](#additional-notes)
6. [References](#references)

---

## Overview

The Firebase Emulator Suite allows you to run Firebase services like Firestore, Auth, Functions, and more on your local machine. This helps with isolated testing and development without touching production data or services.

---

## Prerequisites

- Google Cloud CLI (`gcloud`)  
  → [Install Instructions](https://cloud.google.com/sdk/docs/install-sdk)

- Firebase CLI  
  → Install with:
  ```bash
  npm install -g firebase-tools
  ```

- Access to a Firebase project and permission to export Firestore data.

---

## Importing Firestore Data from Production

To simulate production-like conditions locally, you can export Firestore data from the production environment in Firebase and import it into the local emulator.

### 1. Create or Locate a Cloud Storage Bucket  
Go to the Google Cloud Storage Console and make sure a bucket exists to hold your Firestore export. Create one if needed.

### 2. Export Firestore Data  
Run the following command in your terminal:
```bash
gcloud firestore export gs://<bucket-name>/<export-folder>
```
> Note: Replace `<bucket-name>` and `<export-folder>` with your actual bucket and folder names.

### 3. Navigate to Firebase Functions Directory  
Move into your project’s `functions/` directory:
```bash
`cd functions/`
```

### 4. Download the Firestore Export  
Use `gsutil` to copy the export locally:
```bash
gsutil cp -r "gs://<bucket-name>/<export-folder>" .
```
> ⚠️ Note: The `.` at the end is crucial — it indicates the current directory as the download destination.

### 5. Start Emulator with Imported Data  
Start the Firebase Emulator, specifying the downloaded data directory as import:
```bash
firebase emulators:start --import ./<export-folder>
```

---

## Additional Notes
### Emulator UI
- The emulator provides a local UI at:
  ```
  http://localhost:4000
  ```

- `Auth` and `Functions` support can also be added — this documentation does not contain furher information on this for the time being.

- Some compound queries might fail locally unless mocked or simplified, due Firestore Indexes not being emulated.

---

## References

- [Medium: Importing Firestore Data](https://medium.com/readytowork-org/importing-data-from-firestore-to-emulator-6bcdb83a861c)  
- [Firebase Emulator Suite Docs](https://firebase.google.com/docs/emulator-suite)  
- [Auth Emulator Guide](https://firebase.google.com/docs/emulator-suite/connect_auth)