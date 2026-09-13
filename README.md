# PlantAI — sch_thesis

식물 사진을 이용한 작물 및 질병 분류 Streamlit 프로젝트입니다.

## 다운로드

- **모델 포함 전체 프로젝트:** [릴리스 다운로드](https://github.com/sugarosel/sch_thesis/releases/tag/plant-ai-demo-v1)의 `plant-ai-final-code-demo.zip`
- **소스 코드:** 이 저장소의 `plant-ai-source.zip` (Python 캐시와 모델 가중치 제외)

## 실행

전체 프로젝트 ZIP을 압축 해제한 다음, 최상위의 `facility_crop_fast`와 `facility_disease_fast` 폴더를 `models/` 폴더 안으로 옮기세요. 최종 경로는 `models/facility_crop_fast/best.pt`, `models/facility_disease_fast/best.pt`입니다.

프로젝트 폴더에서 실행합니다.

```powershell
pip install -r requirements.txt
python -m streamlit run app/streamlit_app.py
```

상세 설명은 압축본의 `FRIEND_RUN_GUIDE.md`, `README.md`, `TRAINING_RUNBOOK.md`를 참고하세요. API 키가 필요한 기능은 `.env.example`을 참고해 개인 환경에서 설정하세요.

원본 ZIP은 변경 없이 릴리스에 보관했습니다.
