# 13. UE5.7 + GAS 액션 데모 설계안 — IF 세계관

> 스텔라블레이드 분석 + 언리얼 페스트 2024 발표 기반.
> UE4 우회 기술은 제외하고, UE5.7 네이티브 + GAS로 재설계.

---

## Group A — SKIP (UE4 우회 → UE5 네이티브 대체)

| # | SB (UE4.26) 방식 | UE5.7 대체 | SKIP 이유 |
|---|-----------------|-----------|----------|
| A-1 | 55×55 셀 AI 포지셔닝 + Theta* | EQS + NavMesh + SmartObjects | UE5 EQS가 대폭 개선. 커스텀 pathfinding ROI 낮음 |
| A-2 | CMC 수동 병렬화 (2.8→1ms) | UE5 CMC 자체 async physics | 데모 규모에서 불필요. 엔진 내부 수정 유지보수 부담 |
| A-3 | PhysX 본 제외 + 캡슐 대체 | Chaos Physics | PhysX 자체가 deprecated. Chaos에서 UI로 설정 |
| A-4 | 3ds Max 루트모션 사전 추출 | UE5 네이티브 루트모션 병렬 | Animation Budget Allocator 지원 |
| A-5 | slua_unreal (Lua 스크립팅) | GAS + DataAsset | GAS가 데이터 드리븐 역할 대체 |
| A-6 | 파티클 1ms/프레임 제한 | Niagara Scalability + FXBudget | 엔진 레벨에서 자동 관리 |
| A-7 | GamepadUMGPlugin | **CommonUI** | Steam 출시 + Steam Deck Verified 대응. 게임패드↔키보드 자동 전환 |

---

## Group B — KEEP & ADAPT (GAS 아키텍처로 적응)

### B-1. MoveCurve → GA + 커스텀 AbilityTask
- `GA_MeleeAttack` 내 `UCurveFloat*` 보유
- 커스텀 `AT_ApplyMoveCurve`로 GA에서 재사용
- 넉백: `GE_KnockBack` (Instant) + SetByCaller 커브 기반 거리

### B-2. 이중 타격 판정 → 커스텀 AT_WeaponTrace
- `AT_WeaponTrace`: 이전 프레임 소켓 캐싱 + 보간
- `FGameplayAbilityTargetData_SingleTargetHit`로 결과 전달
- 경사지 보정: AnimBP IK 노드 + `State.Combat.Targeting` 태그 조건부

### B-3. 타격감 3층 → GameplayCue 3종
```
GA_MeleeAttack → Hit 확인 → GE_HitImpact (Instant)
                                    ↓
                             GC_HitImpact (Cosmetic)
                             ├── Layer 1: HitStop (Montage_Pause)
                             ├── Layer 2: BoneShake (커스텀 AnimNode)
                             └── Layer 3: VertexShake (WPO)
```

### B-4. 콤보 체인 → GA 계층 + 커스텀 AT
- `GA_ComboBase` (부모): 콤보 윈도우, 입력 버퍼링
- `AT_WaitComboInput`: 윈도우 내 입력 대기
- Montage Section 분기: `Montage_JumpToSection`

### B-5. 패링/회피 → GE 기반 상태
- `GA_Parry` → 타이밍 판정 → `GE_ParryStun` (Duration 2s)
- `GA_Dodge` → `GE_DodgeInvincible` (Duration 0.2s)
- `GA_PerfectDodge` → `GE_PerfectDodgeReward` (슬로우 + 반격 윈도우)

### B-6. 보스 페이즈 → StateTree + GAS 태그
- Health 임계값 → `GE_PhaseTransition` → Tag `Boss.Phase.2`
- StateTree에서 GameplayTag 조건으로 Phase 분기
- 각 Phase 내 패턴 실행은 BT (하이브리드)

### B-7. 데이터 드리븐 밸런싱 → CurveTable + DataAsset
- `UComboDefinition` (DataAsset): 콤보 시퀀스, 배율, 커브 참조
- `FScalableFloat` + `UCurveTable`로 레벨별 스케일링
- `SetByCaller`로 GE에 동적 값 전달

### B-8. 절단 시스템 → GC에서 비주얼 처리
- `GA_DismemberAttack` → `GE_Dismember` (Instant)
- `GC_Dismember`: `UDynamicMeshComponent` (UE5) 활용
- `Combat.Dismember.LeftArm` 등 태그로 부위 식별

### B-9. 몬스터 역할 분류 → GAS AttributeSet + DataAsset
- 역할별 `AttributeSet` 변형 + GA 세트를 DataAsset으로 관리
- Tag: `Enemy.Role.Attacker/Defender/Ranged/Supporter`

---

## Group C — UE5.7 신규 기능 활용

| # | UE5 기능 | 활용 방식 |
|---|---------|----------|
| C-1 | **Enhanced Input** | `IA_` 입력 액션 + `IMC_` 컨텍스트. 콤보 = 커스텀 InputTrigger |
| C-2 | **Motion Matching** | 로코모션(걷기/달리기)에 적용. 전투는 Montage 유지 (하이브리드) |
| C-3 | **StateTree** | 일반 몬스터 AI 주축. 보스는 StateTree(페이즈) + BT(패턴) 하이브리드 |
| C-4 | **CommonUI** | Steam Deck Verified 필수. 게임패드/키보드 자동 전환, 입력 아이콘 교체 |
| C-5 | **Nanite + Lumen** | 배경 자동 LOD + 실시간 GI. ImpostorBaker 불필요 |
| C-6 | **World Partition** | 반오픈월드 시 자동 스트리밍 |
| C-7 | **Animation Layer Interface** | 무기별 전투 레이어 교체. `ALI_CombatLayer` |
| C-8 | **Gameplay Targeting System** | 락온 대상 선택, AoE 필터링. GAS TargetData 통합 |
| C-9 | **Motion Warping** | 타겟 방향 자동 보정 (SB의 Look-at IK 대체) |

---

## GAS 아키텍처

### GameplayAbility (GA)
| GA | 분류 | 설명 |
|----|------|------|
| `GA_LightAttack` | Combat | 약공격 (콤보 체인) |
| `GA_HeavyAttack` | Combat | 강공격 (차지 가능) |
| `GA_ComboFinisher` | Combat | 콤보 마무리 |
| `GA_BetaSkill_Slot1~4` | Skill | 장착 스킬 슬롯 |
| `GA_Dodge` | Defense | 회피 (i-frame) |
| `GA_PerfectDodge` | Defense | 퍼펙트 닷지 |
| `GA_Parry` | Defense | 패링 |
| `GA_Guard` | Defense | 가드 |
| `GA_HitReaction` | Reaction | 피격 반응 |
| `GA_Death` | Reaction | 사망 |
| `GA_DismemberAttack` | Special | 절단 공격 |
| `GA_BossPhaseTransition` | Boss | 페이즈 전환 |
| `GA_AirCombo` | Combat | 공중 콤보 |

### GameplayEffect (GE)
| GE | 유형 | 설명 |
|----|------|------|
| `GE_Damage_Physical` | Instant | 물리 데미지 (SetByCaller) |
| `GE_KnockBack` | Instant | 넉백 |
| `GE_KnockDown` | Instant+Duration | 넉다운 |
| `GE_Stagger` | Duration | 경직 |
| `GE_DodgeInvincible` | Duration(0.2s) | 회피 무적 |
| `GE_PerfectDodgeReward` | Duration(1.5s) | 퍼펙트 닷지 보상 |
| `GE_ParryStun` | Duration(2s) | 패링 스턴 |
| `GE_ComboCounter` | Duration(3s, Stack) | 콤보 카운트 |
| `GE_WeaponStats` | Infinite | 무기 스탯 |
| `GE_BossPhaseModifier` | Infinite | 보스 페이즈 스탯 |

### GameplayCue (GC)
| GC | 설명 |
|----|------|
| `GC_HitImpact_Light/Heavy/Critical` | 타격감 3층 (HitStop+BoneShake+VertexShake) |
| `GC_ParrySuccess` | 패링 성공 (스파크+시간정지) |
| `GC_PerfectDodge` | 퍼펙트 닷지 (잔상+슬로우) |
| `GC_Dismember` | 절단 비주얼 |
| `GC_BossPhaseTransition` | 보스 전환 연출 |

### AttributeSet
```
UAS_CharacterBase (공통)
├── Health / MaxHealth
├── Stamina / MaxStamina
├── Stagger / MaxStagger (경직 게이지)
└── MoveSpeed

UAS_Combat (전투)
├── AttackPower / DefensePower
├── CriticalRate / CriticalMultiplier
├── ComboCount
└── BetaSkillGauge / MaxBetaSkillGauge

UAS_BossPhase (보스 전용)
├── PhaseThreshold_1 / PhaseThreshold_2
└── EnrageMultiplier
```

### GameplayTag 계층
```
State.Combat.Attacking/Dodging/Parrying/Guarding/HitReaction/Invincible/KnockedDown/Dead
State.Movement.Grounded/Airborne/Locked

Ability.Combo.Light.1~4 / Heavy.1~2 / Finisher / Air.1~3
Ability.Skill.Beta.Slot1~4 / Ultimate
Ability.Defense.Dodge/PerfectDodge/Parry/Guard

Combat.HitDetection.Triangle/Sweep
Combat.MoveCurve.Forward/Airborne/KnockBack
Combat.Dismember.LeftArm/RightArm/LeftLeg/RightLeg/Head

Boss.Phase.1/2/Transition
Boss.Pattern.BlinkDash/CounterCombo/SmashCombo/GrabAttack/Explosion
Boss.Enraged

Enemy.Role.Attacker/Defender/Ranged/Supporter
Enemy.Formation.Surround/Line

GameplayCue.Combat.HitImpact.Light/Heavy/Critical
GameplayCue.Combat.ParrySuccess/PerfectDodge/Dismember
```

---

## 몬스터 AI — StateTree + BT 하이브리드

| AI 대상 | 시스템 | 근거 |
|---------|--------|------|
| 일반 몬스터 | StateTree 단독 | 상태 전이 명확, GameplayTag 네이티브 |
| 엘리트 | StateTree + EQS | 포지셔닝에 EQS, 패턴 선택에 Utility 스코어 |
| **보스** | **StateTree(페이즈) + BT(패턴)** | 페이즈 전환 = StateTree, 복잡한 패턴 시퀀스 = BT |
| NPC | StateTree 단독 | 대화, 순찰, 동행 |

### 보스 AI 구조 (StateTree + BT 하이브리드)
```
StateTree: ST_Boss_Elder
├── [Phase1] — Condition: !Tag(Boss.Phase.2)
│   ├── [Combat_P1] → RunBehaviorTree(BT_ElderP1_Patterns)
│   │   ├── SmashCombo (30%)
│   │   ├── HeadButtCombo (25%)
│   │   ├── JumpAttack (20%)
│   │   └── GrabAttack (25%, distance < 300)
│   └── [Reposition_P1] → EQS: FindAttackPosition
│
├── [PhaseTransition] → GA_BossPhaseTransition
│
├── [Phase2] — Condition: Tag(Boss.Phase.2)
│   └── [Combat_P2] → RunBehaviorTree(BT_ElderP2_Patterns)
│       ├── BlinkDash + CounterCombo (신규)
│       ├── DashStamp / Explosion
│       └── KillRoutine (처형기)
│
└── [HitReaction] — Global (우선순위 최고)
    └── Tag(State.Combat.Staggered) → PlayMontage
```

---

## 프로젝트 폴더 구조

```
IF_ActionDemo/
├── Source/IF_ActionDemo/
│   ├── AbilitySystem/          — GAS 핵심
│   │   ├── Abilities/          — GA_ 클래스
│   │   ├── AbilityTasks/       — AT_ 커스텀 (WaitCombo, MoveCurve, WeaponTrace)
│   │   ├── Attributes/         — AS_ 클래스
│   │   ├── Effects/            — ExecCalc, MMC
│   │   └── GameplayCues/       — GC_ C++ 핸들러
│   ├── Character/              — 캐릭터 베이스/플레이어/적/보스
│   ├── Combat/                 — CombatComponent, HitImpact, Dismember, MoveCurve
│   ├── AI/                     — AIController, StateTree Task/Condition, EQS
│   ├── Input/                  — Enhanced Input 설정
│   └── GameplayTags/           — 중앙 태그 선언
│
├── Content/
│   ├── Blueprints/AbilitySystem/  — GA/GE/GC 블루프린트
│   ├── Art/                       — Characters, Environments, FX, UI
│   ├── Animation/                 — Montages, AnimBP, AnimLayers
│   ├── Data/Combat/               — MoveCurves, ComboDefinitions, CurveTables
│   ├── Input/                     — IA_, IMC_ 에셋
│   ├── Levels/                    — TrainingRoom, Demo_Stage
│   └── UI/                        — CommonUI 위젯 (HUD, BossHP, DamageNumber, LockOn)
│
└── Plugins/KawaiiPhysics/         — 머리카락/장신구 물리
```

---

## Steam 출시 + Steam Deck OLED 대응

### 타겟 하드웨어
| 항목 | Steam Deck OLED | PC (최소) | PC (권장) |
|------|----------------|----------|----------|
| CPU | Zen 2 4C/8T | Ryzen 5 3600 | Ryzen 5 5600X |
| GPU | RDNA 2 8CU (1.6TF) | GTX 1060 | RTX 3060 |
| RAM | 16GB LPDDR5 | 16GB | 16GB |
| 해상도 | 1280×800 | 1920×1080 | 2560×1440 |
| 프레임 | 60fps 목표 (90Hz 가능) | 60fps | 60fps+ |
| 디스플레이 | 7.4" HDR OLED | — | HDR 지원 |
| 스토리지 | NVMe SSD | SSD 권장 | SSD |

### Scalability 프리셋 설계

```
[SteamDeck]            [PC_Medium]          [PC_Epic]
Nanite: Off (LOD 폴백)  Nanite: On           Nanite: On
Lumen: Off (SSGI)       Lumen: On (SW)       Lumen: On (HW RT)
VSM: Off (CSM)          VSM: On              VSM: On
TSR: Quality            TSR: Quality         TSR: Off (네이티브)
FSR: Quality (우선)     FSR: Optional        DLSS: Optional
Shadow: Medium          Shadow: High         Shadow: Epic
FX: Medium              FX: High             FX: Epic
해상도: 720p→800p 업스케일  1080p               1440p+
```

**Steam Deck 핵심**: AMD GPU이므로 **FSR 우선**, DLSS 없음. TSR+FSR 조합으로 720p 렌더 → 800p 출력.

### 렌더링 필수 설정

**RHI — Vulkan 필수**
- SteamOS = 리눅스 기반 → **Vulkan이 기본 타겟**
- DX12는 Proton 변환 레이어를 거치므로 Vulkan 네이티브가 성능/안정성 우위
- `DefaultEngine.ini`: `DefaultGraphicsRHI=Vulkan`

**TSR 내부 해상도**
- 800p 출력 기준 내부 렌더 **50~67%** (400~536p) → TSR로 보정
- Quality 프리셋: 67%, Balanced: 58%, Performance: 50%

**VRS (Variable Rate Shading)**
- Steam Deck RDNA 2가 하드웨어 VRS 지원
- 화면 주변부 셰이딩 빈도↓ → GPU 부하 10~15% 절감
- UE5 `r.VRS.Enable=1`, 중앙부 1x1, 주변부 2x2 또는 4x4

**Lumen 최적화 전략**
- Steam Deck: **Software Ray Tracing** 모드 (HW RT 불가)
- `LumenSceneDetail` / `FinalGatherQuality` 최소로
- 또는 **베이크드 라이팅 혼합**: 정적 GI + 동적 광원만 Lumen
- 배터리 효율 우선이면 Lumen OFF + SSGI 폴백

**Nanite 메모리 대역폭 주의**
- LPDDR5 공유 메모리 → Nanite 액터 과다 시 대역폭 병목
- 전략적 배치: 복잡한 배경 메시에만 Nanite, 단순 메시는 전통 LOD

### PSO 캐싱 — 셰이더 스터터링 방지 (필수)
```
리눅스/Proton 환경에서 셰이더 컴파일 → 프레임 버벅임 발생
→ Pipeline State Object (PSO) 사전 캐싱으로 해결

1. 개발 중 PSO 수집: r.ShaderPipelineCache.Enabled=1
2. 출시 빌드에 PSO 캐시 번들 포함
3. 첫 실행 시 백그라운드 컴파일 (로딩 화면 활용)
```

### 리눅스 크로스 컴파일
- 윈도우 개발 환경에서도 **Linux cross-compilation 툴체인** 설치 필수
- UE5: `Engine/Extras/ThirdPartyNotUE/SDKs/HostLinux/` 참조
- CI/CD에 Linux 빌드 포함 → Steam에 Windows + Linux 동시 배포

### Steam Deck 개발 환경
```
1. Steam Deck Devkit Tool — PC↔Deck 네트워크 연결, 빌드 직접 전송
2. Performance Overlay Level 4 — CPU/GPU 병목 실시간 모니터링
3. MangoHud — 프레임타임 그래프, VRAM 사용량 추적
4. 배터리 프로파일링 — 50Whr 기준 목표 플레이타임 설정 (3시간+)
```

### HDR 지원
- Steam Deck OLED = HDR 지원
- UE5 `r.HDR.EnableHDROutput=1`
- 톤매핑: ACES + HDR 밝기/대비 조절 옵션 제공
- SDR 모니터 폴백 자동 처리

### Steamworks 통합
| 기능 | UE5 구현 |
|------|---------|
| **OnlineSubsystem** | `OnlineSubsystemSteam` 플러그인 활성화 |
| **업적** | GAS GameplayTag 기반 업적 트리거 (`Achievement.FirstBoss` 등) |
| **클라우드 세이브** | `ISaveGameSystem` + Steam Remote Storage |
| **Steam Input** | Enhanced Input과 병행. Steam Controller 레이아웃 제공 |
| **Steam Deck 호환** | 게임 속성에서 "Steam Deck 호환" 태그 설정 |
| **리치 프레즌스** | 현재 스테이지/보스 정보 Steam 프로필에 표시 |

### Steam Deck Verified 체크리스트
- [x] **입력**: 모든 기능 게임패드로 조작 가능 (CommonUI)
- [x] **디스플레이**: 1280×800 기본 지원, UI 스케일링
- [x] **원활성**: 런처 없이 바로 게임 시작
- [x] **시스템 지원**: Proton 호환 (UE5 네이티브 양호)
- [ ] 최소 폰트 크기 9pt (7.4" 화면 가독성)
- [ ] 가상 키보드 불필요 (게임패드 전용 플로우)
- [ ] 전원 관리 (50Whr 배터리, 저전력 프리셋)

### UI — CommonUI + GAS 바인딩
```
CommonUI 위젯 스택:
├── WBP_HUD (항상 표시)
│   ├── HP바 (AsyncTaskAttributeChanged → Health)
│   ├── 스태미나바 (→ Stamina)
│   ├── 베타게이지 (→ BetaSkillGauge)
│   ├── 콤보카운트 (→ ComboCount)
│   └── 입력 아이콘 (CommonUI 자동 전환: 키보드↔게임패드)
├── WBP_BossHP (보스 전투 시)
│   ├── 보스 HP바 + 페이즈 표시
│   └── 보스 이름
├── WBP_DamageNumber (풀링)
│   └── 데미지 숫자 팝업 (GC에서 트리거)
├── WBP_LockOn (락온 시)
│   └── 타겟 마커
└── WBP_PauseMenu (일시정지)
    └── ActivatableWidget 스택으로 관리
```

### 프로젝트 폴더 추가
```
Content/
├── ...기존 구조...
├── Config/
│   ├── SteamDeck/              — Deck 전용 Scalability 프리셋
│   └── DefaultEngine.ini       — OnlineSubsystemSteam 설정
└── Plugins/
    ├── KawaiiPhysics/
    └── OnlineSubsystemSteam/   — Steamworks SDK
```

---

## Trade-offs

| 결정 | 장점 | 단점 |
|------|------|------|
| GAS 사용 | 표준화, 시프트업에 역량 어필 | 프레임 단위 제어 오버헤드 |
| StateTree 주축 | UE5 최신 어필, Tag 연동 | "왜 BT 안 썼나" 면접 대비 필요 |
| MoveCurve→AT | SB 핵심 패턴 재현 | 커스텀 AbilityTask 개발 비용 |
| 절단 시스템 | 기술력 어필 극대화 | 구현 난이도, 스코프 초과 위험 |
| Motion Matching | 최신 기술 어필 | 실험적, 데이터 필요 |
