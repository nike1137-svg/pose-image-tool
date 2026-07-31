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

## metadata/

이미지 8장 각각의 생성 조건이다. 프롬프트, 시드, 시드 계산 규칙, 모델 이름, 스텝, guidance, control scale, 해상도, 라이브러리 버전이 들어 있다. 이 json 하나만 있으면 같은 이미지를 다시 만들 수 있다.
