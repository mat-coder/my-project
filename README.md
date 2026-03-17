# 📄 Financial Status Certificate — Template Filler

## What This Does

A Streamlit web app that takes a **Government of Telangana Financial Status Certificate** `.docx` template and lets users fill in all variable fields through a clean UI, then downloads a fully filled `.docx` file with the original formatting 100% preserved.

---

## Files in This Package

| File | Purpose |
|------|---------|
| `app.py` | Streamlit app (UI + document generation logic) |
| `Mohd_Aslam__1_.docx` | Original `.docx` template — **do not modify** |
| `test_app.py` | Automated test suite for document generation logic |
| `requirements.txt` | Python dependencies |
| `README.md` | This file |

---

## Agent Instructions — Step-by-Step Setup

### Step 1 — Prerequisites

Ensure Python 3.9+ is installed:
```bash
python3 --version
```

### Step 2 — Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 3 — Place the Template

Make sure `Mohd_Aslam__1_.docx` is in the **same directory** as `app.py`.

```
your_folder/
├── app.py
├── Mohd_Aslam__1_.docx   ← template, keep as-is
├── requirements.txt
└── README.md
```

### Step 4 — Run the App

```bash
streamlit run app.py
```

The app opens at `http://localhost:8501` in your browser.

### Step 5 — Run the Tests (Optional)

```bash
python -m unittest test_app.py
```

---

## How to Use the App

The UI has **5 sections**:

### 1️⃣  Letter & Reference Details
- **Lr. No. (Certificate)** — e.g. `G/478/2026`
- **Lr. No. (Note File)** — e.g. `G/478/2025`
- **Dated** — e.g. `.03.2026`
- **Tahsildar Lr. No.** and **Dated**
- **Mandal** — e.g. `Balapur Mandal`

### 2️⃣  Applicant Details
- Full Name, Relationship to Deceased (S/o / D/o / W/o), Deceased Name, Full Address

### 3️⃣  Deceased Employee Details
- Deceased's father name, Designation, Old Office, Renamed Office, Date of Death

### 4️⃣  Further Enquiry Details
- Pension status, Movable Property, Other Income, Financial Position (dropdown), Remarks

### 5️⃣  Family Members Table
- Click **➕ Add** or **➖ Remove** to flexibly include as many family members as needed. The template dynamically expands or shrinks safely.
- For each: Name, Age, Relation, Marital Status, Occupation, Education, Income/Month
- **Validation**: All critical fields act as required fields. Specific elements like 'Age' are strictly validated to be actual digits before a certificate is generated.

### Generate
Click **🖨️ Generate Filled Certificate (.docx)** → download button appears instantly.

---

## What Is Preserved (Template Integrity)

| Element | Preserved? |
|---------|-----------|
| Page layout & margins | ✅ Yes |
| All fonts & font sizes | ✅ Yes |
| Table borders & shading | ✅ Yes |
| Header text (Govt of Telangana etc.) | ✅ Yes |
| Boilerplate legal text | ✅ Yes |
| Both certificate + note file sections | ✅ Yes |
| All enquiry table rows | ✅ Yes |

Only the **variable data values** change — nothing else.

---

## How It Works (Technical)

1. **`python-docx`** opens the original template without modifying it on disk.
2. A list of `(old_text, new_text)` pairs replaces tokens across all paragraphs and table cells using a run-safe replacement (handles text split across Word runs).
3. The two **family member tables** and two **enquiry tables** are detected by their header row content and filled cell-by-cell.
4. The filled document is saved to an in-memory buffer and streamed to the browser as a download.

---

## Customising for Other Templates

To adapt this app to a different `.docx` template:

1. Replace `Mohd_Aslam__1_.docx` with your template file and update `TEMPLATE_PATH` in `app.py`.
2. Update the `replacements` list in `generate_doc()` with your own `(old_text, new_text)` pairs.
3. Adjust the table-filling logic if your table has different columns.
4. Update the Streamlit UI fields to match your new variables.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `FileNotFoundError: Mohd_Aslam__1_.docx` | Ensure the `.docx` file is in the same folder as `app.py` |
| Text not replaced in output | Check exact spelling/spacing in the `replacements` list matches the template |
| Extra blank family rows in output | Reduce "Number of family members" to match actual rows in your template |
| App won't start | Run `pip install -r requirements.txt` again |
