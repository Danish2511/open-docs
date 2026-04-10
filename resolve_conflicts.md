# 🔹 1. Conflicts: `staging_dv → staging_ts`

### Step 1: Identify branches

* Source → `staging_dv`
* Target → `staging_ts`

---

### Step 2: Create new branch from source

```bash
git checkout staging_dv
git checkout -b resolve_conflicts_dv
```

---

### Step 3: Merge target into new branch

```bash
git merge staging_ts
```

---

### Step 4: Resolve conflicts

* Open VS Code
* Fix conflict markers
* Remove:

```
<<<<<<< HEAD
=======
>>>>>>>
```

---

### Step 5: Add resolved files

```bash
git add <filename>
```

---

### Step 6: Commit

```bash
git commit -m "Resolved conflicts between staging_ts and staging_dv"
```

---

### Step 7: Push

```bash
git push origin resolve_conflicts_dv
```

---

### Step 8: Raise PR

👉 From `resolve_conflicts_dv` → `staging_dv`

---

# 🔹 2. Conflicts: `staging_ts → staging_qa`

### Step 1: Identify branches

* Source → `staging_ts`
* Target → `staging_qa`

---

### Step 2: Create new branch from source

```bash
git checkout staging_ts
git checkout -b resolve_conflicts_ts
```

---

### Step 3: Merge target into new branch

```bash
git merge staging_qa
```

---

### Step 4: Resolve conflicts (VS Code)

---

### Step 5: Add resolved files

```bash
git add <filename>
```

---

### Step 6: Commit

```bash
git commit -m "Resolved conflicts between staging_qa and staging_ts"
```

---

### Step 7: Push

```bash
git push origin resolve_conflicts_ts
```

---

### Step 8: Raise PR

👉 From `resolve_conflicts_ts` → `staging_ts`

---

# 🔹 3. Conflicts: `staging_qa → main`

### Step 1: Identify branches

* Source → `staging_qa`
* Target → `main`

---

### Step 2: Create new branch from source

```bash
git checkout staging_qa
git checkout -b resolve_conflicts_qa
```

---

### Step 3: Merge target into new branch

```bash
git merge main
```

---

### Step 4: Resolve conflicts (VS Code)

---

### Step 5: Add resolved files

```bash
git add <filename>
```

---

### Step 6: Commit

```bash
git commit -m "Resolved conflicts between main and staging_qa"
```

---

### Step 7: Push

```bash
git push origin resolve_conflicts_qa
```

---

### Step 8: Raise PR

👉 From `resolve_conflicts_qa` → `staging_qa`

---
