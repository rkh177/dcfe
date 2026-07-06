# Dynamic Cognitive Field Equation (DCFE)

Reference implementation for the paper:  
"Dynamic Cognitive Field Equation for Artificial Personality "  
Kihyun Ryu, Gawon Lee


---

## Overview

This repository provides a minimal reference implementation to illustrate the core theoretical predictions of the Dynamic Cognitive Field Equation (DCFE). The code is intended as a demonstrative instance to showcase interpretive bifurcation; it is not intended to serve as a definitive architecture or a standardized industrial specification.

All numerical constants and mapping dictionaries within the code are parameters set for demonstration purposes. The optimization and rigorous quantification of these constants to align with diverse linguistic contexts and emotional nuances remain a key task for future research.

---

## Scope and Limitations

This implementation employs simplified computational choices to demonstrate the feasibility of the proposed theory. While these choices are sufficient for illustrating the qualitative mechanisms, they do not imply a definitive or unique form of the framework.

| Step | Implementation Choice |
|------|----------------------|
| Context Mapping | Keyword-based activation used as a proxy |
| Field State Update | Simple first-order accumulation and decay |
| Weight Calculation | Multiplicative combination of activated fields |
| Collapse Selection | Selection of the attractor with maximum force |
| Constant Settings | Manual configuration for functional demonstration |

The DCFE remains methodologically agnostic — it can be integrated 
with various model scales and through diverse intervention methods, 
such as hidden-state modulation. This work focuses on exploring the 
qualitative alignment between the DCFE and linguistic output, rather 
than prescribing a fixed technical solution or optimized performance 
for specific tasks.

---


## DCFE + TinyLlama Integration

This section describes a minimal implementation to demonstrate that 
the emotional bifurcation results calculated by the DCFE can modulate 
the output direction of a language model. The implementation pipeline 
consists of three stages:

1. DCFE Bifurcation Calculation: Input field values are passed to 
`DCFE_engine.compute_weights()` to calculate weights for each emotion. 
The emotion with the maximum weight is selected as the dominant emotion.

2. Emotion-Vocabulary Mapping: The selected dominant emotion 
activates a specific set of predefined words. While the vocabulary 
list remains static, the specific emotional category selected varies 
dynamically at the moment of execution.

3. Logit Bias-based Generation: A constant bias is added to the 
logits of tokens corresponding to the selected vocabulary. TinyLlama 
then generates a response through standard autoregressive decoding.

the language model itself is not responsible for determining the interpretive direction; the DCFE engine determines 
the dominant emotion prior to language generation.

TinyLlama-1.1B was specifically selected because higher-parameter 
LLMs often exhibit strong internal linguistic constraints that resist 
external logit-level intervention, making it difficult to steer 
output toward the intended emotional valence for validation purposes.

---

## Relational T_ij Extraction

`DCFE_Tij_extract.py` provides an exploratory probe for examining how relational context can modulate cognitive mass \(T_{ij}\). The purpose of this script is not to claim a complete extraction method for \(T_{ij}\), but to illustrate why cognitive mass should be treated as relational and tensorial rather than as a simple scalar weight.

In DCFE, the same event does not acquire the same cognitive mass in every context. For example, a killing event changes its interpretive force depending on the object and context of the act: killing an innocent person, killing a murderer, killing the murderer of one’s family, killing a criminal, or killing for revenge. These cases all retain a positive pull toward the `crime` attractor, but the strength of that pull varies according to relational and normative context.

The script estimates this variation by comparing hidden-state representations from TinyLlama against a simplified `crime` attractor. A bounded severity modulation is then applied to preserve the sign structure: the event is not pushed away from crime, but its pull toward crime is strengthened or weakened by the object and context.

This reflects the central claim of DCFE:

> Cognitive mass does not arise from the stimulus alone, but from the relation among subject, object, history, and normative context.

Therefore, \(T_{ij}\) must be represented as a relational tensor. A scalar value cannot distinguish between structurally different cases such as “killing an innocent person” and “killing the murderer of one’s family,” even though both contain the same surface-level action.

The current implementation uses manually defined semantic anchors such as `crime`, `innocent`, `guilty`, `justice`, and `revenge`. These anchors are only illustrative. Future work should replace them with empirically calibrated cognitive masses derived from larger models, neural manifold analysis, behavioral data, or multimodal signals.

The resulting values should be interpreted as candidate signals for \(T_{ij}\), not as definitive moral measurements.

이 모듈은 DCFE(동적 인지장 방정식)에서 제안하는 관계적 인지 질량($T_{ij}$)이 LLM의 은닉 상태 기하학 내에 실재함을 보여주는 참조 구현입니다. 본 자료는 특정 수치의 절대성을 주장하기보다, 기하학적 신호의 존재와 그 한계를 명확히 규정하여 DCFE 프레임워크의 필요성을 역설하는 데 목적이 있습니다.

1. 인지적 중력의 관측: 심각도 역전 현상
추출된 데이터(Revenge(0.63) > Family's Killer(0.46))는 LLM이 행위의 도덕적 정당성보다 의미론적 범죄 밀도(Semantic Crime Density)에 정직하게 반응함을 보여줍니다.

시사점: 이는 LLM 내부에 특정 개념(예: Crime)으로 이끄는 '인지적 중력'이 존재함을 뜻하며, 이를 수치화된 '질량'으로 다룰 수 있다는 물리적 근거를 제공합니다.

2. 기하학적 관성과 다중 중력 중첩의 필연성데이터에서 관측되듯, 문맥 변화에 따른 실제 거리($T_{ij}$)의 변동폭은 의사결정을 완전히 뒤집을 만큼 크지 않습니다.
 - 신호의 한계: LLM 내부의 기하학적 신호만으로는 명확한 해석적 분기(Bifurcation)를 자율적으로 생성하기에 에너지가 부족합니다.DCFE의 당위성: 이러한 미세 신호는 단독으로 쓰이기보다, 사회적 규범($T_{norm}$)이나 정서적 중량($T_{affect}$)과 같은 외부 인지 중력과 중첩되어야 합니다.
 - DCFE는 이 다중 중력의 중첩을 통해 시스템이 특정 해석으로 기하학적 붕괴(Geometric Collapse)를 일으키도록 유도하는 유일한 경로입니다.
   
3. 왜 텐서($T_{ij}$) 구조인가?본 추출 과정은 '누가 누구에게($i \to j$)'라는 관계적 맥락이 추가됨에 따라 인지적 좌표가 체계적으로 이동함을 실증합니다.
- 관계적 가변성: 대상(Object)의 변화가 인지 질량의 크기와 방향을 결정하는 핵심 변수임을 입증함으로써, $T$를 단순 스칼라가 아닌 관계 지향적 텐서로 정의해야 한다는 본문의 주장을 뒷받침합니다.

   
4. 연구적 가이드라인: 변조와 재귀적 형성
추출된 $T_{ij}$는 고정된 결과값이 아닌, 인격 형성을 위한 원시 신호(Raw Signal)입니다.
- 증폭과 변조: 향후 과제는 이 미세한 신호를 어떻게 증폭/변조하여 결정론적인 인격적 반응을 이끌어낼 것인가에 있습니다.
- 재귀적 인격 베이스: 이 거리를 재귀적 인격 형성 과정에 편입시켜, 사회적 규범의 질량으로 통제되는 '선한 기저 베이스(Pro-social Baseline)'를 형성하는 과정이 본 방정식이 지향하는 궁극적인 인공 인격의 모습입니다.

![Relational T_ij Modulation](modulated_objects.png)

---

## P6/P7 Scenarios

Two distinct field configurations were applied to an identical prompt 
to evaluate the system's responsiveness.

| Scenario | Field Configuration | Selected Emotion |
|----------|-------------------|-----------------|
| P6 | cortisol: 1.5, dopamine: 0.65, social norm: 0.0 | Justice |
| P7 | cortisol: 1.5, dopamine: 0.65, social norm: 100.0 | Crime |

In P6, where the social norm field is not activated, vocabulary 
related to Justice is reinforced. In P7, a high social norm value 
geometrically foregrounds Crime as the dominant attractor, 
redirecting the collapse away from Justice. In both scenarios, the 
input prompt and language model remain identical; the only variable 
is the dominant emotion calculated by the DCFE engine.

---

## License
MIT License

---

*This work originated from an exploratory research inquiry into the 
structural conditions required for persistent interpretive consistency 
in artificial cognitive systems.*
