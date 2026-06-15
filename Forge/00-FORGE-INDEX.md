---
tags: [forge, index]
---

# 🔧 FORGE — 지식 위키

> [[Dashboard/00-HOME|홈]] | [[ARENA/BOJ-문제집|문제집]] | **FORGE** | [[SHIP/00-SHIP-INDEX|SHIP]] | [[Dashboard/Job-Tracker|취업]]

> 게임 프로그래머를 위한 핵심 지식 베이스. `Ctrl+N` → FORGE-Wiki 템플릿으로 추가.

---

## 최근 추가

```dataview
TABLE WITHOUT ID file.link as "노트", file.folder as "분류", file.cday as "생성일"
FROM "FORGE"
WHERE file.name != "00-FORGE-INDEX" AND file.name != "00-INDEX"
SORT file.cday DESC
LIMIT 5
```

---

## 📂 카테고리

### 면접 필기 ⭐ (현재 집중)
```dataview
LIST FROM "FORGE/면접필기" SORT file.name ASC
```

### C++
```dataview
LIST FROM "FORGE/CPP" SORT file.name ASC
```

### 알고리즘
```dataview
LIST FROM "FORGE/Algorithm" SORT file.name ASC
```

### 아키텍처
```dataview
LIST FROM "FORGE/Architecture" SORT file.name ASC
```

### 네트워크
```dataview
LIST FROM "FORGE/Network" SORT file.name ASC
```

### 엔진 — UE5
```dataview
LIST FROM "FORGE/Engine" SORT file.name ASC
```

### Godot
```dataview
LIST FROM "FORGE/Godot" SORT file.name ASC
```

### 게임 분석
```dataview
LIST FROM "FORGE/Game-Analysis" SORT file.name ASC
```

### AI 워크플로우
```dataview
LIST FROM "FORGE/AI-Workflow" SORT file.name ASC
```

---

## 📊 통계

```dataview
TABLE WITHOUT ID file.folder as "카테고리", length(rows) as "노트 수"
FROM "FORGE"
WHERE file.name != "00-FORGE-INDEX" AND file.name != "00-INDEX"
GROUP BY file.folder
SORT length(rows) DESC
```

---

> **관련**: [[FORGE/Godot/00-INDEX|Godot 위키(STS2)]] · [[FORGE/Game-Analysis/Nakwon/00-INDEX|낙원 분석]]
