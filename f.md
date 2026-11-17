These are all the steps on your Mac, in order. Run them from the project folder.

**1. Save any local changes you have** (skip this if `git status` shows nothing):

```bash
git status
```

```bash
git stash
```

**2. Add the GitHub repo where my changes are** (once only; if it says the remote already exists, carry on):

```bash
git remote add claude https://github.com/ziyadbarq/techhub.git
```

**3. Fetch the changes and switch to the `dev` branch:**

```bash
git fetch claude dev
```

```bash
git checkout dev
```

**4. Merge GitLab's latest first, in case someone else pushed, then my changes:**

```bash
git pull gitlab dev
```

```bash
git pull claude dev
```

If a merge conflict appears, stop and send me the output.

**5. Confirm the changes arrived.** The first line should be `7becd19 feat(workflows): real file attachments for File fields`:

```bash
git log --oneline -3
```

**6. Install the new packages** (`pdfjs-dist` was added):

```bash
npm install
```

**7. Apply the database changes** (the new roles, and attachments without a request):

```bash
npx prisma migrate deploy
```

```bash
npm run db:generate
```

**8. Restart the dev server with a clean cache.** Stop the current server first (Ctrl+C), then:

```bash
npm run dev:clean
```

**9. Push to GitLab** so the pipeline can run:

```bash
git push gitlab dev
```

**10. Restore your local changes** (only if you ran `git stash` in step 1):

```bash
git stash pop
```

**After the steps, check:**
- **Roles:** 6 roles (System Administrator, Viewer, Asset Manager, Asset Officer, CMDB Manager, Workflow Officer).
- **Knowledge Base:** no "New folder" at the third level.
- **Access Services:** "Download the Excel template" link inside the import box.
- **Forms:** the File field shows a drag-and-drop area.

When you deploy to GCP or OCI, the migrations run automatically at pod startup. No extra step is needed there.
