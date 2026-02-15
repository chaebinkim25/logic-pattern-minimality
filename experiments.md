# 🧪 Experiment Log: Finding the Minimal Viable Model
> **Goal:** 각 과제(Topic)를 해결할 수 있는 "이론상 최소 모델(Minimum Description Length)"을 실험적으로 증명한다.

## 0. 실험 환경 (Common Setup)
- **Input:** 10x10 Grid (Flattened to 100-dim vector $x \in \{0, 1\}^{100}$)
- **Loss Function:** CrossEntropyLoss (Softmax 포함)
- **Optimizer:** SGD or Adam (Learning Rate 고정)
- **Success Criteria:**
    - Phase A (Logic): Test Accuracy **100%** (단 1개의 오답도 허용 X)
    - Phase B (Pattern): Test Accuracy **≥ 95%** (노이즈/변형 고려)

---

## Phase A: 좌표와 논리 (Logic & Geometry)
**가설:** "위치 찾기, 개수 세기, 정렬 등 고정된 규칙은 **Hidden Layer가 없는 선형 모델(Linear)**로 완벽히 풀 수 있다."

### 🔬 실험 1: 선형 모델의 한계 (Linear Solver)
* **Model A0 (Linear):** Input(100) $\to$ Linear(100 outputs) $\to$ Softmax
* **Model A1 (MLP):** Input(100) $\to$ Linear(64) $\to$ ReLU $\to$ Linear(100) $\to$ Softmax

| Topic | Description | Model | # Params | Accuracy | Pass/Fail | 비고 (Insight) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | 점 1개 위치 찾기 | **A0 (Linear)** | $100 \times 100$ | 00.0% | [ ] | 1:1 매핑, 자명함 |
| **2** | 점 2개 중 왼쪽(위쪽) | **A0 (Linear)** | $100 \times 100$ | 00.0% | [ ] | 페널티 가중치로 논리 구현 확인 |
| **3** | 점 0~2개 (None 포함) | **A0 (Linear)** | $100 \times 101$ | 00.0% | [ ] | Bias 조절로 Threshold 구현 확인 |
| **4** | 두 점 정렬 (Min/Max) | **A0 (Dual Head)**| $100 \times 200$ | 00.0% | [ ] | Head 1(Min), Head 2(Max) 독립 학습 |

> **결론:** Topic 1~4는 은닉층이 [ 필요하다 / 필요없다 ].

---

## Phase A (Topic 5): 노이즈 내성 (Stress Test)
**가설:** "노이즈가 심해지면 선형 모델(A0)은 붕괴하고, 비선형 모델(A1)이나 Conv 모델이 필요해지는 **임계점(Critical Point)**이 존재한다."

* **Task:** Topic 2 (왼쪽 점 찾기) + Random Noise (Flip probability $p$)
* **Noise Spec:** $p \in \{0.0, 0.05, 0.1, 0.2\}$ (각 픽셀이 $p$ 확률로 반전)

| Noise ($p$) | Model A0 (Linear) Acc | Model A1 (MLP 1-Hidden) Acc | 승자 (Winner) |
| :---: | :---: | :---: | :--- |
| **0.00** | 100% | 100% | Tie (A0 승 - 더 간단함) |
| **0.05** | 00.0% | 00.0% | |
| **0.10** | 00.0% | 00.0% | |
| **0.20** | 00.0% | 00.0% | |

> **관찰:** 노이즈 $p=$ \_\_\_% 이상부터 선형 모델의 정확도가 급격히 하락함.

---

## Phase B: 패턴 인식 (Pattern Recognition)
**가설:** "패턴의 **위치 불변성(Translation Invariance)**을 해결하려면, Linear 모델은 비효율적이며 **Conv(합성곱)**가 구조적으로 유리하다."

### 🔬 실험 2: 구조 대결 (Architecture Search)
* **Task:** 6가지 패턴(ㅡ, ㅜ, ㅠ, ㅢ, ㅟ, ㅒ) 분류 (위치 랜덤, 10x10)
* **Model B0 (Linear):** Flatten $\to$ Linear(6)
* **Model B1 (CNN):** Conv2d(kernel=3, filters=6) $\to$ GlobalMaxPool $\to$ Softmax

| Model Structure | # Params | Epochs to Converge | Final Accuracy | 특징 |
| :--- | :--- | :--- | :--- | :--- |
| **B0 (Linear Only)** | $100 \times 6 = 600$ | - | 00.0% | 위치마다 패턴을 따로 외워야 함 (Overfitting 위험) |
| **B1 (Conv 1-Layer)**| $9 \times 6 = 54$ | - | 00.0% | **파라미터 1/10 수준.** 위치 상관없이 탐지 가능 |

> **최종 결론:**
> 1. 논리 연산(Phase A)은 [ \_\_\_\_\_\_ ] 모델로 충분하다.
> 2. 시각적 패턴(Phase B)은 [ \_\_\_\_\_\_ ] 구조가 압도적으로 효율적이다.
> 3. 따라서 **최소 모델(Minimal Model)**은 문제의 성격(Logic vs Pattern)에 따라 달라진다.
