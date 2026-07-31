# 테스트 결과 파일

`pose_tool.ipynb` 를 실행해 나온 결과다. 모든 파일은 노트 11번 셀에서 내려받은 `pose_results.zip` 에서 옮겼다.

## 기본 2쌍

| 파일 | 내용 |
|---|---|
| `pose_01.png` | 참조로 쓴 포즈 사진 (양손 허리). 노트가 실제로 먹인 512×512 크기 |
| `skeleton_01.png` | 위 사진에서 OpenPose가 뽑은 관절 |
| `output_01.png` | 같은 자세로 만든 결과 (astronaut 프롬프트) |
| `compare_01.png` | `참조 \| 스켈레톤 \| 결과` 나란히 비교 |
| `pose_02.png` | 참조로 쓴 포즈 사진 (양팔 만세) |
| `skeleton_02.png` | 위 사진의 관절 |
| `output_02.png` | 같은 자세로 만든 결과 (astronaut 프롬프트) |
| `compare_02.png` | 비교 그림 |

## 같은 포즈에 프롬프트만 바꾼 것 (pose_01)

| 파일 | 프롬프트 |
|---|---|
| `output_01_knight.png` / `compare_01_knight.png` | 중세 기사 |
| `output_01_watercolor.png` / `compare_01_watercolor.png` | 수채화 |

## 같은 프롬프트에 포즈만 바꾼 것

| 파일 | 내용 |
|---|---|
| `output_02_knight.png` / `compare_02_knight.png` | pose_02(양팔 만세) + 중세 기사 |

## 포즈 추종 강도만 바꾼 것 (pose_01 + astronaut 고정)

| 파일 | control scale |
|---|---|
| `compare_scale_0.5.png` | 0.5 |
| `compare_scale_1.0.png` | 1.0 |
| `compare_scale_1.5.png` | 1.5 |

## 재실험 — 대조군과 시드 고정

처음 실험의 결론 네 개를 뒤집은 실험들이다. 자세한 내용은 [상위 README의 재실험 절](../README.md#재실험--대조군과-시드-고정) 참고.

| 파일 | 내용 |
|---|---|
| `ablation_pose01.png` / `ablation_pose02.png` | 대조군 몽타주 — `참조 \| 스켈레톤 \| 조건 끔 \| 조건 켬` |
| `ablation_pose01_scale0.0.png` 등 4장 | 위 몽타주의 개별 원본 |
| `seed_variation.png` | 시드만 바꾼 4장 몽타주 (조건 고정) |
| `seedtest_11/22/33/44.png` | 개별 원본 |
| `scale_fixedseed.png` | 시드 고정, 강도만 0.0~1.5 로 바꾼 4장 몽타주 |
| `scalefix_0.0/0.5/1.0/1.5.png` | 개별 원본 |
| `hand_condition.png` | 손 조건 몽타주 — `손 포함 스켈레톤 \| 몸통만 \| 손 조건 켬` |
| `skeleton_02_with_hands.png` | 손 관절이 포함된 스켈레톤 |
| `hand_body_only.png` / `hand_included.png` | 개별 원본 |

> 이 실험들의 이미지에는 json 이 없다. 조건(시드·강도·프롬프트)이 노트 13번 절 코드에 그대로 적혀 있어 그것으로 재현된다.

## metadata/

이미지 8장 각각의 생성 조건이다. 프롬프트, 시드, 시드 계산 규칙, 모델 이름, 스텝, guidance, control scale, 해상도, 라이브러리 버전이 들어 있다. 이 json 하나만 있으면 같은 이미지를 다시 만들 수 있다.
