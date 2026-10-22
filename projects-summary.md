# 프로젝트 요약 (포트폴리오)

> 기준일: 2026-10-22 · 작성 기준: 각 저장소의 `ProjectSettings`, `Packages/manifest.json`, `Assets` 폴더 구조, 문서(README/HANDOFF 등), `git log`
> 커밋 수는 `git log --all --author=leeyunsu9003` 기준 (괄호 안은 merge 제외 수)
> 스토어 링크는 bundle id 로 만든 **추정 링크**이며, 실제 공개 여부는 따로 확인이 필요합니다.

## 한눈에 보기

| 프로젝트 | 장르 | 상태 | Unity | 핵심 스택 | 내 역할 |
|---|---|---|---|---|---|
| **SexyCardRPG** | 카드 수집·턴제 배틀 RPG | 출시 (v1.0.31 추정) | 6000.3.9f1 | Firebase, LevelPlay, IAP, Tenjin, SOAP | 전투·도감·팀·포획 연출·저장·Firebase·VFX 연결 (UI·스토리 제외 전반) |
| **Typing Clash** | 실시간 타이핑 대전 (몬스터·마법 전략) | 개발 중 (Steam) | 6000.3.9f1 | Photon Fusion 2, Steamworks, Firebase Functions | 인게임 전투·네트워크 (UI 는 다른 팀원) |
| **Super Slap Arena** (SlapFightArena) | 물리 래그돌 따귀 파티 대전 | 개발 중 | 6000.3.9f1 | Photon Fusion 2 (Host), Steamworks.NET | **1인 개발** |
| **RaidMyDungeon** | 던전 공략 액션 + 던전 제작 | 개발 중 | 6000.3.9f1 | 결정적 시뮬레이션, .NET 8 검증 도구, Addressables, UniTask | 전투 시뮬·전투 화면·던전 에디터 |
| **DontWatchAds** | 캐주얼 (광고 컨셉) | 개발 초기 | 6000.6.4f1 | LevelPlay, AdMob, IAP, Tenjin | git 기준 커밋 없음 |

---

## 1. SexyCardRPG

- **장르 / 소개**: 카드(몬스터)를 모으고 팀을 짜서 턴제로 싸우는 카드 수집 RPG. 야생 몬스터 포획, 도감, 마을/지역 진행, 클론(다른 유저 기록) 대전이 있음.
- **상태**: 출시 운영 중으로 추정 — `bundleVersion 1.0.31`, `AndroidBundleVersionCode 1031`
  - Android ID: `com.molayong.sexycardrpg`
  - Google Play (추정): https://play.google.com/store/apps/details?id=com.molayong.sexycardrpg
- **엔진**: Unity 6000.3.9f1, URP 17.3
- **기술 스택**
  - 백엔드: Firebase (Auth/Google 로그인, Firestore 보안 규칙 `FirebaseRules/` — bank·chat·clone·daycare·nickname·notice·pass, 푸시, 크래시 진단)
  - 수익화/분석: Unity LevelPlay, Google Mobile Ads, Unity IAP 5.4, Tenjin SDK, Google Play Review
  - 기타: UniTask, Obvious SOAP(ScriptableObject 이벤트/변수), Easy Save 3, Anti-Cheat Toolkit(ACTk), DOTween, Unity Localization (25개 언어)
- **핵심 시스템 (코드 근거)**
  1. **턴제 배틀** — `Manager/04_Battle/`: `BattleTurnController`(partial: Resolve/Effects/Status/Field/Defeat/Input), `BattleDamage`, `BattleTypeChart`, `BattleStatus`·`BattleStatusRules`, `BattleWeather`, `BattleSkillCopy`
  2. **배틀 규칙 분리** — `IBattleRule` + `Dungeon/Gym/League/Monster/User BattleRule`, AI(`SimpleBattleAI`, `AIBattleRule`, `CloneBattleAI`)
  3. **포획 & 연출** — `CaptureFormula`, `FX/ChainCaptureFX`, `CaptureHaptics`, `CatchTutorialController`, `CardPackOpenFX`
  4. **도감·팀·저장** — `IndexManager`/`CardIndexCell`, `TeamManager`, `SaveManager`·`SaveStorage`·`CloudSave`
  5. **클론 대전** — `Clone/`: `CloneSnapshot`, `CloneService`, `CloneRating`, `PlayStyleRecorder`/`PlayStyleProfile`(유저 플레이 성향 기록 → AI 대전)
- **내 기여** — 커밋 **314개** (merge 제외 242), 2026-06 ~ 2026-10, `.cs` 기준 약 +28,400 / −3,000 줄
  - **UI·스토리를 제외한 게임 로직 전반 담당**: 전투, 도감(Index), 팀, 포획 연출, 저장(로컬/클라우드), Firebase 연동, VFX 연결 등
  - 가장 많이 작업한 파일: `BattleTurnController`(57), `BattleManager`(52), `TeamManager`(20), `BattleBoard`(18), `CardManager`(18), `BattlePlayerInput`(15), `SaveManager`(11)
  - 직접 처음 만든 파일 예: `BattleTurnController`, `BattleTypeChart`, `SaveManager`, `CloudSave`, `ChainCaptureFX`, `CloneSnapshot`, `CloneBattleAI`, `FirebaseManager`
  - 테스트용 에디터 메뉴: `BattleTestMenu`, `CardTestMenu`, `BagTestMenu`, `IndexTestMenu`
- **공통 모듈 관계**: 저장소 안에 SexyEngine 패키지 참조는 없음(`manifest.json` 확인). 다국어 정의·번역 CSV 형식이 SlapFightArena 로 옮겨짐.

---

## 2. Typing Clash (저장소: Typing-Crash)

- **장르 / 소개**: 단어를 타이핑해서 몬스터를 소환·마법을 쓰고 상대 성(넥서스)을 부수는 실시간 타이핑 대전. 싱글(CPU)·멀티·키즈(키보드 가이드) 모드.
- **상태**: 개발 중 — `bundleVersion 0.1.0`, PC(Steam) 타깃
  - Steam App ID `3562030` → 스토어 (추정): https://store.steampowered.com/app/3562030
  - Android ID 는 템플릿 기본값(`com.UnityTechnologies...`) 그대로라 구글 플레이 링크 없음
- **엔진**: Unity 6000.3.9f1, URP 17.3
- **기술 스택**
  - 네트워킹: **Photon Fusion 2** (Shared 모드), Unity Multiplayer Services, Multiplayer Play Mode, ParrelSync
  - 플랫폼/서버: Steamworks.NET + Heathen Steamworks (업적), **Firebase Cloud Functions** (`functions/index.js` — `steamAuth` HTTPS 함수로 Steam 인증), Firebase (문장 DB 업로드 스크립트 다국어)
  - 기타: UniTask, SOAP, Burst/Collections, Newtonsoft Json, Unity Localization
- **핵심 시스템 (코드 근거)**
  1. **몬스터 AI·스폰** — `InGame/Monster/`: `MonsterAI`, `MonsterSpawner`, `MonsterStat`, `NexusStat`, `Projectile`
  2. **네트워크 동기화** — `Photon/FusionNetworkManager`, `PlayerManager`, `NetworkMonsterSync`, `Managers/SessionManager`
  3. **타이핑 마법** — `InGame/Magic/`: `MagicTypingPanel`, `MagicEffect`, `Meteor`, `IceFieldArea`, `ToxicCloudArea`, `BurningFloorArea`
  4. **채팅 → 몬스터 해석** — `ChatManager`, `ChatMonsterResolver`(입력한 단어로 유닛 결정)
  5. **경제·맵 오브젝트** — `GoldManager`, `MinerAI`(일꾼 채굴), `BarrierManager`, `CastleController`, `PlayerBuilding`, 싱글용 `SinglePlayAI`
- **내 기여** — 커밋 **278개** (merge 제외 228), 2026-03 ~ 2026-10
  - **인게임 전투/네트워크 담당, UI 는 다른 팀원 담당**
  - 주요 폴더: `Script/InGame`(527 파일 변경), `Script/Managers`(238), `Script/Photon`(71), `Script/Editor`(51)
  - 많이 작업한 파일: `MonsterAI`(58), `SessionManager`(50), `InGameManager`(31), `PlayerBuilding`(29), `NetworkMonsterSync`·`ChatMonsterResolver`(23), `GoldManager`·`SinglePlayAI`(22)
  - 커밋 메시지 예: "포톤 임포트 및 로컬 채팅기능 추가", "멀티플레이 완성", "몬스터 1티어 2티어 골드티어 추가", "일꾼이 금 캐는 기능 추가", "중복 메소드 soap으로 통일"
- **공통 모듈 관계**: SexyEngine 참조 없음. 이 프로젝트의 Photon Fusion SDK·폰트·언어 정의가 SlapFightArena 로 옮겨짐.

---

## 3. Super Slap Arena (저장소: SlapFightArena) — 1인 개발

- **장르 / 소개**: 액티브 래그돌 캐릭터로 따귀를 때려 상대 팀을 쓰러뜨리는 온라인 파티 대전 (Gang Beasts 스타일).
- **상태**: 개발 중 — `bundleVersion 0.1.0`, Windows 빌드 있음(`Builds/`)
  - `steam_appid.txt` = `480`(Steam 테스트용 Spacewar ID) → 아직 자체 스토어 페이지 없음
  - Android ID 는 템플릿 기본값 → 구글 플레이 링크 없음
- **엔진**: Unity 6000.3.9f1, URP 17.3
- **기술 스택**
  - 네트워킹: **Photon Fusion 2 (Host 모드)** — 물리를 한 곳에서 계산하기 위해 Host 모드 선택 (`docs/HANDOFF.md`)
  - 플랫폼: Steamworks.NET (친구 초대·초대 수락 참가), Multiplayer Play Mode
  - 기타: Unity Localization (23개 언어), Addressables, EditMode 자동 테스트(18개), SteamPipe 업로드 도구
- **핵심 시스템 (코드 근거)**
  1. **액티브 래그돌** — `Ragdoll/ActiveRagdoll`, `ActiveRagdollConfig`, `ConfigurableJointExtensions`
  2. **따귀 전투** — `Combat/SwingLogic`, `SwingTracker`, `SlapJudge`, `HealthLogic` (모으기 시간에 따라 데미지·쿨다운 증가)
  3. **경기 흐름·모드** — `Match/MatchManager`, `MatchPhase`, `MatchRules`, `ModeCatalog`, `BotBrain`(봇)
  4. **온라인 세션** — `Network/SessionService`, `RoomCode`, `NetworkedPlayer`, `SlapNetInput` (빠른 매칭·방 코드·방장 이어받기)
  5. **맵 기믹** — `Maps/`: `ZeroGravityCycle`, `BlackHole`, `JumpPad`, `MazeMap`, `SoccerField`, `CrownMode` 등, 아이템 에어드랍(`Items/`)
- **내 기여** — 커밋 **31개 전부 본인** (저장소 author 1명), 2026-09-25 ~ 2026-10-16
  - 기획·코드·빌드 전 과정을 혼자 진행
  - 주요 폴더: `Scripts/Maps`, `Player`, `Match`, `Network`, `Ragdoll`, `Combat`, 에디터 도구(`SlapFightMapBuilder`, `SlapFightBuild`)
- **공통 모듈 관계**: SexyEngine 참조 없음. Typing-Crash 에서 Fusion SDK·폰트, SexyCardRPG 에서 번역 CSV 형식을 가져옴(네트워크 코드는 구조만 참고해 새로 작성).

---

## 4. RaidMyDungeon (저장소: LaidMyDungeon)

- **장르 / 소개**: 영웅(검사·아처·파이터·메이지)으로 던전을 공략하는 액션 + 직접 던전을 만드는 던전 제작 게임. 리플레이 검증을 위한 결정적(deterministic) 전투 시뮬레이션 구조.
- **상태**: 개발 중 — `Server/README.md` 상 백엔드(계정·던전·경매장·리플레이 검증 워커)는 다음 단계 예정
  - Android ID: `com.yonggames.raidmydungeon`
  - Google Play (추정): https://play.google.com/store/apps/details?id=com.yonggames.raidmydungeon
- **엔진**: Unity 6000.3.9f1, URP 17.3
- **기술 스택**
  - 시뮬레이션: 고정소수점 `FP`, `DetRandom`, `StateHash`, 60Hz 틱 — `LMD.Sim.asmdef` 를 `noEngineReferences` 로 Unity 의존 차단
  - 서버/도구: .NET 8 `Server/LMD.SimCheck` (기획 CSV 가져오기 검사, 결정성·리플레이 점검, Unity 테스트와 같은 해시 확인)
  - 기타: Addressables, Cinemachine, UniTask, SOAP, Firebase, LevelPlay, EasyGridBuilder Pro
- **핵심 시스템 (코드 근거)**
  1. **결정적 전투 시뮬** — `Sim/Core`(`FP`, `DetRandom`, `StateHash`), `Sim/World`(`SimWorld`, `Combat`, `Hero`, `Projectile`)
  2. **리플레이 검증** — `Sim/Replay/Replay`, `ScenarioInput`, `Tests/EditMode/SimDeterminismTests` (Unity Mono ↔ .NET 결과 일치)
  3. **데이터 테이블 파이프라인** — `Sim/Data/CsvTable`, `TableCatalog`, `GameData` (기획 CSV → 게임 표)
  4. **전투 화면** — `Battle/SimRunner`, `EntityView`, `BattleCamera`, `BossIntro` (시뮬 결과를 표시하는 뷰 계층)
  5. **던전 에디터** — `Dungeon/DungeonEditor`, `DungeonEditorUI`, `DungeonLayoutJson`, `Sim/Dungeon/WorldGen`
- **내 기여** — 커밋 **38개** (merge 제외 30), 2026-07-30 ~ 2026-10-22
  - 주요 폴더: `Script/Sim`(209 파일 변경), `Script/Battle`(111), `Script/Dungeon`(31), `Script/Economy`(26), `Script/Kits`(24)
  - `FP`, `DetRandom`, `Replay`, `SimWorld`, `Combat`, `SimRunner`, `DungeonEditor`, `Server/LMD.SimCheck` 를 처음 작성하고 이후 수정도 본인만 함
  - 커밋 메시지 예: "0단계: 결정적 전투 시뮬 뼈대 · CSV 가져오기 · 리플레이 검증", "1단계: 검사 평타 3타 · 띄우기 · 공중 콤보 · 피격 반응 · 히트스톱 · 근접 몬스터 AI", "파이터 아처 검사 메이지 추가함"
  - 다른 팀원: 기획 문서·프로토타입(`Assets/1_Design`, `Prototype_v2`), 에셋 임포트
- **공통 모듈 관계**: SexyEngine 참조 없음.

---

## 5. DontWatchAds

- **장르 / 소개**: "광고를 보지 마세요" 컨셉의 캐주얼 게임 (HTML 프로토타입 `Assets/prototype/dont-watch-ads.html` 기준, 세부 장르는 추정 불가)
- **상태**: 개발 초기 — `bundleVersion 0.0.01`, 첫 커밋 2026-10-07
  - Android ID: `com.yonggames.dontwatchads`
  - Google Play (추정): https://play.google.com/store/apps/details?id=com.yonggames.dontwatchads
- **엔진**: Unity 6000.6.4f1, URP 17.6
- **기술 스택**: Unity LevelPlay, Google Mobile Ads, Unity IAP 5.4, Tenjin, Firebase(Remote Config), Google Sign-In, Unity Localization, Anti-Cheat(`Security/AntiCheatGuard`)
- **핵심 시스템**: `00_Core/Monetization`(광고·IAP 설정), `00_Core/IAP`(IAP 래퍼), `RemoteConfigManager`, `LocalizationHelper`·`KoreanJosa`
- **내 기여**: `git log --author=leeyunsu9003` 결과 **0개** (커밋 author 는 다른 팀원 1명)

---

## 6. 기타 Sexy 시리즈 (이력서 기재 내용과 동일 — 상세 분석 생략)

SexyEngine (공통 모듈) · sexy3tilematch · SexyArrow · SexyBlockJam · SexyDotConnect · SexyGravityBall · SexyHexJam · SexyPhotoPuzzle · SexyPicturePuzzle

> 참고: `D:\` 에서 git 저장소를 찾을 때 `sexy3tilematch`, `SexyGravityBall` 이름의 저장소는 확인되지 않았습니다.
