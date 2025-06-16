# LLM-grounded-Diffusion

### LLM-grounding 기반 객체 삽입 알고리즘 개발

- 기존 이미지 생성 모델에서의 객체 삽입은 대부분 텍스트 프롬프트 기반 attention peak를 기준으로 위치를 정하는 방식으로 진행되어 와서 정확한 in-context 위치 지시가 불가능하고 threshold peak 기준 포인트 선정의 불안정성이 있었음
- 주변 scene context에 따라 자연스럽고 정합성 높은 객체 삽입을 위해 “LLM기반 grounding과 RAG(Retrieval-Augmented Generation)을 활용한 2단계 방식의 객체 삽입 알고리즘”을 개발하고자 함



### 모델 구성 요소
	
#### 1. 이미지 이해 기반 Grounding Reasoning
- 이미지 내 scene context, 물리적 affordance, 객체 관계 등을 이해하여 삽입 가능한 영역과 비삽입 영역을 분리함. 
- 추론한 정보를 바탕으로 삽입 가능 위치 후보 맵 생성(binary 또는 score 형태)하고 이 grounding mask는 이후 RAG+LLM 기반 위치 추론의 soft prior로 활용함

#### 2. LLM-RAG 기반 위치 선정 및 삽입
- 위 단계에서 생성된 grounding prior를 soft constraint로 사용하여 RAG를 통해 유사 scene에서의 객체 배치 retrieval함
- LLM기반 위치 설명 추출 후 Text-Image 정합성 평가 + Visual affordance consistency loss를 결합함
- Top-K 삽입 위치 중 Best 후보를 선택하여 객체 삽입 수행함
