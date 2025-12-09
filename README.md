<!-- ======================= -->
<!--   GFGSolutions README   -->
<!-- ======================= -->

<p align="center">
  <!-- Animated banner (place your animated GIF at assets/banner.gif) -->
  <img src="assets/banner.gif" alt="GFGSolutions Banner" style="max-width:100%; border-radius:12px;" />
</p>

<h1 align="center">🚀 GFGSolutions – Daily DSA, GFG POTD & LeetCode Hub</h1>
<p align="center">Clean • Optimized • Consistent • Interview Ready</p>

---

<p align="center">
  <img src="https://img.shields.io/badge/Lessons-Daily-brightgreen?style=for-the-badge" alt="Daily" />
  <img src="https://img.shields.io/badge/GFG-POTD-blue?style=for-the-badge" alt="GFG POTD" />
  <img src="https://img.shields.io/badge/LeetCode-Daily-orange?style=for-the-badge" alt="LeetCode"/>
  <img src="https://img.shields.io/badge/Contribute-PRs_welcome-9cf?style=for-the-badge" alt="Contribute"/>
</p>

---

## 📘 What this repo contains
This repository contains daily Problem-Of-The-Day (POTD) solutions from **GeeksforGeeks** and **LeetCode**, organized by date and language. Each solution includes a short explanation and complexity analysis where applicable.

---

## 📊 Live Counters (auto-updated)
<!-- COUNTERS_START -->
**Total Problems (C++ / Python / Java):** 0 / 0 / 0  
**Total Solutions:** 0
<!-- COUNTERS_END -->

> These counters are refreshed daily by a GitHub Action `.github/workflows/update-readme.yml`.

---

---

## 🧩 Daily workflow
1. Fetch GFG / LeetCode POTD  
2. Draft approach & complexity  
3. Implement solution (C++ / Python / Java)  
4. Add explanation + tests  
5. Commit & push

---

## 🔧 How counters are computed
The GitHub Action counts tracked files with `git ls-files` (so it only counts files tracked in the repo). It counts:
- `*.cpp` → C++
- `*.py` → Python
- `*.java` → Java  
and writes totals into the README inside the `<!-- COUNTERS_START -->` / `<!-- COUNTERS_END -->` block.

---

## 📌 Useful links
- GFG POTD: https://practice.geeksforgeeks.org/problem-of-the-day  
- LeetCode: https://leetcode.com/problemset/all/

---

## 🙌 Support
If this repo helps you, please star ⭐ it — it motivates continued daily updates.

---

<p align="center">Built with consistency & curiosity — <strong>GFGSolutions</strong></p>

### File: .github/workflows/update-readme.yml

name: Update README Counters

# runs daily at 00:00 UTC (change cron if you want a different time)
on:
  schedule:
    - cron: '0 0 * * *'
  workflow_dispatch: {}

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Count C++ / Python / Java files
        id: count_files
        run: |
          cpp_count=$(git ls-files '*.cpp' | wc -l | tr -d ' ')
          py_count=$(git ls-files '*.py' | wc -l | tr -d ' ')
          java_count=$(git ls-files '*.java' | wc -l | tr -d ' ')
          total=$((cpp_count + py_count + java_count))
          echo "cpp=$cpp_count" >> $GITHUB_OUTPUT
          echo "py=$py_count" >> $GITHUB_OUTPUT
          echo "java=$java_count" >> $GITHUB_OUTPUT
          echo "total=$total" >> $GITHUB_OUTPUT

      - name: Show counts (debug)
        run: |
          echo "C++ count: ${{ steps.count_files.outputs.cpp }}"
          echo "Python count: ${{ steps.count_files.outputs.py }}"
          echo "Java count: ${{ steps.count_files.outputs.java }}"
          echo "Total: ${{ steps.count_files.outputs.total }}"

      - name: Update README counters
        run: |
          CPP=${{ steps.count_files.outputs.cpp }}
          PY=${{ steps.count_files.outputs.py }}
          JAVA=${{ steps.count_files.outputs.java }}
          TOTAL=${{ steps.count_files.outputs.total }}

          # Use perl (with DOTALL) to replace the block between markers
          perl -0777 -pe "s/<!-- COUNTERS_START -->.*?<!-- COUNTERS_END -->/<!-- COUNTERS_START -->\n**Total Problems (C++ \\/ Python \\/ Java):** $CPP \\/ $PY \\/ $JAVA  \n**Total Solutions:** $TOTAL\n<!-- COUNTERS_END -->/s" README.md > README.tmp
          mv README.tmp README.md
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add README.md
          if git commit -m "chore(readme): update language counters [skip ci]" ; then
            git push
          else
            echo "No changes to commit"
          fi

      - name: Complete
        run: echo "README counters updated (if changes present)"

