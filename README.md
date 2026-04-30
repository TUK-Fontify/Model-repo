# 영어 폰트 → 한글 글리프 자동 생성 GAN

학습된 모델을 불러와서 사용하는 코드입니다
---

## 환경 세팅

**Python 3.10** 설치 후 진행

```bash
git clone {레포 주소}
cd {프로젝트 폴더}

python -m venv venv
venv\Scripts\activate

pip install -r requirements.txt
```

**필수 파일 2개 — 팀원한테 받기:**
- `checkpoints/epoch_0200.pt`
- `NanumGothic.ttf` → 프로젝트 루트에 넣기

---

## 실행

파이참에서 `infer.py` 실행 → TTF 파일 선택 → `new_font_test/{폰트이름}/output/` 에 결과 저장

---

## 백엔드 연동

```python
from infer import GlyphGAN

# 서버 시작 시 한 번만
gan = GlyphGAN("./checkpoints/epoch_0200.pt", "./NanumGothic.ttf")

# 요청마다
results = gan.generate_from_ttf("./업로드된폰트.ttf")
# {"가": PIL.Image, "나": PIL.Image, ...}

# PIL → bytes
import io
buf = io.BytesIO()
results["가"].save(buf, format="PNG")
image_bytes = buf.getvalue()
```

> ⚠️ `GlyphGAN()` 초기화는 서버 시작할 때 한 번만, 요청마다 새로 만들지 말 것

---

## 폴더 구조

```
프로젝트/
├── infer.py
├── NanumGothic.ttf
├── requirements.txt
├── checkpoints/
│   └── epoch_0200.pt
└── new_font_test/            # 자동 생성
    └── {폰트이름}/
        ├── input/
        └── output/
```

---

## 생성 대상 한글 14자

```
가 나 더 려 모 부 쇼 야 져 쵸 켜 튜 프 히
```
