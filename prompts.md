# 테스트에 쓴 프롬프트 모음

모든 프롬프트는 영어로 작성했다. 한글 프롬프트는 SD1.5의 텍스트 인코더(CLIP)가 영어로 학습돼 있어 거의 반영되지 않는다.

## 공통 조건

전 실험에서 아래 값을 고정했다. 이 값을 바꾸면 결과가 달라지므로 비교가 성립하지 않는다.

| 항목 | 값 |
|---|---|
| base model | `stable-diffusion-v1-5/stable-diffusion-v1-5` |
| controlnet | `lllyasviel/control_v11p_sd15_openpose` |
| steps | 20 |
| guidance scale | 7.5 |
| size | 512 × 512 |
| control scale | 1.0 (9번 셀 실험만 예외) |
| scheduler | UniPCMultistepScheduler |

### negative prompt (전 실험 공통)

```
lowres, bad anatomy, extra limbs, deformed hands, watermark, text, blurry
```

### 시드

난수를 쓰지 않고 조건 문자열에서 계산했다.

```python
seed = int(hashlib.sha256(f"{pose_id}|{prompt_id}|{scale}".encode()).hexdigest()[:16], 16) % 2**32
```

파이썬 내장 `hash()` 는 프로세스마다 값이 달라져 재현이 깨지므로 쓰지 않았다. 이 규칙 덕분에 같은 조건은 언제 어디서 돌려도 같은 시드가 나온다.

## 참조 포즈

| id | 자세 | 출처 |
|---|---|---|
| `pose01` | 양손을 허리에 얹고 정면으로 선 자세 | [Pexels](https://www.pexels.com) |
| `pose02` | 양팔을 위로 크게 벌려 든 자세 (만세) | [Pexels](https://www.pexels.com) |

Pexels 라이선스는 상업적 이용을 허용하고 출처 표기 의무가 없다. 참조 사진의 얼굴·옷·배경은 결과에 전달되지 않고 관절 위치만 조건으로 쓰인다.

## 프롬프트 세트

### A. `astronaut` — 사실적 인물

```
an astronaut in a white spacesuit, on the surface of mars, cinematic lighting
```

의도: 인체가 두꺼운 옷에 덮인 상태에서도 관절 방향이 유지되는지 본다.

### B. `knight` — 사실적 인물, 다른 재질

```
a medieval knight in polished steel armor, stone castle courtyard, dramatic light
```

의도: A와 인체 구조는 같고 재질·배경만 다르게 해서, 프롬프트를 바꿨을 때 자세가 흔들리지 않는지 본다.

### C. `watercolor` — 회화 스타일

```
watercolor painting of a dancer, soft pastel colors, visible paper texture
```

의도: 사실적 인물이 아닌 회화 스타일에서도 포즈 조건이 작동하는지 본다. 스타일이 바뀌면 윤곽이 흐려져 포즈 추종이 약해지는 경우가 있다.

## 실험 조합

### 실험 1 — 같은 포즈, 프롬프트만 변경

| 조합 | 포즈 | 프롬프트 |
|---|---|---|
| `pose01_astronaut` | pose01 | A |
| `pose01_knight` | pose01 | B |
| `pose01_watercolor` | pose01 | C |

기대: 자세는 그대로, 내용과 재질만 바뀐다.

### 실험 2 — 같은 프롬프트, 포즈만 변경

| 조합 | 포즈 | 프롬프트 |
|---|---|---|
| `pose01_astronaut` | pose01 | A |
| `pose02_astronaut` | pose02 | A |
| `pose01_knight` | pose01 | B |
| `pose02_knight` | pose02 | B |

기대: 내용은 그대로, 자세만 바뀐다.

### 실험 3 — 포즈 추종 강도만 변경

프롬프트 A, 포즈 pose01 고정. `control_scale` 만 `0.5 / 1.0 / 1.5` 로 변경.

| 조합 | control scale |
|---|---|
| `pose01_astronaut_cs0.5` | 0.5 |
| `pose01_astronaut_cs1.0` | 1.0 |
| `pose01_astronaut_cs1.5` | 1.5 |

기대: 값이 커질수록 자세는 정확해지고 그림은 뻣뻣해진다.

## 프롬프트를 쓰면서 알게 된 것

- **자세를 프롬프트로 지정하는 문구를 넣지 않았다.** `arms raised` 같은 표현을 넣으면 포즈가 반영된 것이 ControlNet 때문인지 프롬프트 때문인지 구분되지 않는다. 포즈는 전부 스켈레톤 조건에만 맡겼다.
- **negative prompt를 전 실험에서 동일하게 유지했다.** 여기를 조합마다 바꾸면 비교 대상이 두 개가 되어 관찰이 무의미해진다.
