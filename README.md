# Plant Diagnosis MVP

PyTorch + Streamlit 기반의 대학 프로젝트용 MVP 템플릿입니다.

## 1) 프로젝트 목표
- 식물 사진 업로드
- 종 분류(species classifier)
- 질병/건강 상태 분류(disease classifier)
- 날씨 기반 맞춤 추천(rule-based)

## 2) 폴더 구조
```
plant ai/
├─ app/
│  └─ streamlit_app.py
├─ configs/
│  ├─ disease_train.yaml
│  ├─ inference.yaml
│  └─ species_train.yaml
├─ data/
│  ├─ raw/
│  └─ processed/
├─ models/
│  ├─ disease/
│  └─ species/
├─ scripts/
│  └─ predict.py
├─ src/
│  ├─ data/
│  │  ├─ datasets.py
│  │  └─ transforms.py
│  ├─ inference/
│  │  └─ pipeline.py
│  ├─ models/
│  │  ├─ backbones.py
│  │  └─ losses.py
│  ├─ services/
│  │  ├─ recommendation.py
│  │  └─ weather.py
│  ├─ training/
│  │  ├─ engine.py
│  │  ├─ train_classifier.py
│  │  ├─ train_disease.py
│  │  └─ train_species.py
│  └─ utils/
│     ├─ config.py
│     ├─ io.py
│     └─ seed.py
├─ .env.example
└─ requirements.txt
```

## 3) 데이터 준비
`configs/species_train.yaml`, `configs/disease_train.yaml`에서 경로를 수정하세요.

- `data.format: imagefolder`일 때:
  - `train_dir`, `val_dir` 사용
- `data.format: csv`일 때:
  - `csv_path`, `image_root`, `path_col`, `label_col`, `split_col` 사용

PlantNet-300K zip README의 구조가 다르면, 위 설정만 바꿔서 맞출 수 있습니다.

imagefolder 원본만 있을 경우 train/val 분할:
```bash
python scripts/split_imagefolder.py --src_dir data/raw/plantnet/images --out_dir data/processed/plantnet --val_ratio 0.2
python scripts/split_imagefolder.py --src_dir "data/raw/archive (2)/PlantVillage/PlantVillage" --out_dir data/processed/plantvillage --val_ratio 0.2
```

## 4) 설치
```bash
pip install -r requirements.txt
```

## 5) 학습
```bash
python -m src.training.train_species --config configs/species_train.yaml
python -m src.training.train_disease --config configs/disease_train.yaml
```

## 6) 학습 후 평가 리포트 생성
```bash
python -m src.training.evaluate_classifier --config configs/disease_train.yaml --checkpoint models/disease/best.pt --output_dir reports/disease_eval
```

생성 파일:
- `reports/disease_eval/metrics.json`
- `reports/disease_eval/classification_report.json`
- `reports/disease_eval/confusion_matrix.csv`
- `reports/disease_eval/top_misclassifications.csv`

## 7) Streamlit 실행
```bash
streamlit run app/streamlit_app.py
```

## 8) OpenWeather API
`.env` 파일 생성 후 API 키 입력:
```
OPENWEATHER_API_KEY=your_api_key
```

AI 기반 관리 추천을 사용하려면 OpenAI API 키도 함께 입력:
```
OPENAI_API_KEY=your_openai_api_key
OPENAI_RECOMMENDATION_MODEL=gpt-4.1-mini
```

`OPENAI_API_KEY`가 없거나 호출에 실패하면 앱은 자동으로 기본 규칙 기반 추천을 표시합니다.

## 9) 현재 데이터 EDA/분할 결과
- EDA 요약: `EDA_PlantVillage.md`
- 클래스 분포: `reports_class_distribution.csv`
- train/val 분할 분포: `reports_split_distribution.csv`
- disease 학습 데이터 경로:
  - `data/processed/plantvillage/train`
  - `data/processed/plantvillage/val`

## 10) MVP 이후 확장
- ViT 등 백본 교체: `src/models/backbones.py`
- 추천 고도화: `src/services/recommendation.py`
- 사용자 히스토리 저장: DB 레이어 추가
- 모바일 앱 확장: FastAPI 래핑 + 앱 연동
