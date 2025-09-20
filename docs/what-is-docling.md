## Docling 라이브러리 소개

- **Docling**은 IBM Research Zurich의 AI for Knowledge 팀에서 시작한 오픈소스 문서 처리 라이브러리입니다. 다양한 문서 포맷을 파싱하고, 특히 고급 PDF 이해 기능을
  제공하며, 생성형 AI 생태계와의 원활한 통합을 지원합니다.
- 즉, **Docling**은 문서 처리를 위한 포괄적인 솔루션으로, 특히 RAG(Retrieval-Augmented Generation) 파이프라인이나 문서 기반 AI 애플리케이션 개발에 유용한 도구입니다.
- [공식 홈페이지](https://docling-project.github.io/docling/)
- [Docling GitHub](https://github.com/docling-project/docling)

## 📌 주요 특징

### 1. **다양한 문서 포맷 지원**

- **문서**: PDF, DOCX, PPTX, XLSX, HTML
- **이미지**: PNG, TIFF, JPEG 등
- **오디오**: WAV, MP3 (ASR 모델 활용)
- 통합된 DoclingDocument 형식으로 표현

### 2. **고급 PDF 이해 기능**

- 페이지 레이아웃 분석
- 읽기 순서 파악
- 테이블 구조 인식
- 코드 블록 감지
- 수식 처리
- 이미지 분류

### 3. **다양한 내보내기 형식**

- Markdown
- HTML
- DocTags (새로운 태그 기반 형식)
- 무손실 JSON

### 4. **AI 프레임워크 통합**

- LangChain
- LlamaIndex
- Crew AI
- Haystack

### 5. **시각 언어 모델(VLM) 지원**

- SmolDocling (256M 파라미터의 경량 모델)
- Apple Silicon에서 MLX 가속 지원

## 🚀 설치 및 사용법

### 설치

```bash
pip install docling
```

- macOS, Linux, Windows 지원
- x86_64, arm64 아키텍처 모두 호환

### 기본 사용 예시

```python
from docling.document_converter import DocumentConverter

# URL 또는 로컬 경로 지원
source = "https://arxiv.org/pdf/2408.09869"
converter = DocumentConverter()
result = converter.convert(source)

# Markdown으로 내보내기
print(result.document.export_to_markdown())
```

### CLI 사용

```bash
# 기본 변환
docling https://arxiv.org/pdf/2206.01062

# VLM 모델 활용
docling --pipeline vlm --vlm-model smoldocling https://arxiv.org/pdf/2206.01062
```

## 🔮 향후 개발 계획

- 메타데이터 추출 (제목, 저자, 참조, 언어)
- 차트 이해 (막대그래프, 파이차트, 선그래프 등)
- 복잡한 화학 구조 이해 (분자 구조)

## 🏢 프로젝트 정보

- **라이센스**: MIT
- **호스팅**: LF AI & Data Foundation 프로젝트
- **기술 보고서**: [arXiv:2408.09869](https://arxiv.org/abs/2408.09869)
- **문서**: [docling-project.github.io/docling](https://docling-project.github.io/docling/)

## 💡 주요 차별점

1. **로컬 실행**: 민감한 데이터 처리를 위한 에어갭 환경 지원
2. **OCR 지원**: 스캔된 PDF와 이미지 처리
3. **플러그앤플레이**: 다양한 AI 프레임워크와 즉시 통합 가능
4. **통합 표현**: 모든 문서를 일관된 형식으로 변환

## LangChain의 Docling 연동인 langchain_docling.DoclingLoader

### 다루는 대상: DoclingLoader의 생성자 인자

- file_path
- converter (optional)
- convert_kwargs (optional)
- export_type (optional)
- md_export_kwargs (optional)
- chunker (optional)
- meta_extractor (optional)

### 인자별 상세 설명

#### 1) file_path: source as single str (URL or local file) or iterable thereof

- 값: 문자열 하나(로컬 경로 또는 URL) 혹은 그 이터러블(list, tuple 등).
- 역할: 변환할 입력 문서 경로(들)를 지정합니다. 여러 개를 넘기면 각 문서를 차례로 처리하여 LangChain Document 리스트를 반환합니다.
- 비고: 공식 예시도 단일 문자열 또는 리스트/이터러블 형태를
  보여줍니다 [[1]](https://github.com/docling-project/docling-langchain) [[4]](https://python.langchain.com/docs/integrations/providers/docling/).

#### 2) converter (optional): any specific Docling converter instance to use

- 값: docling.document_converter.DocumentConverter 또는 호환되는 Docling 컨버터 인스턴스.
- 역할: PDF 등 입력을 Docling의 내부 표현(DLDocument)으로 변환하는 파이프라인을 책임집니다. 기본값이 있으며, 직접 생성해 넘기면 OCR 설정, 디바이스(GPU/MPS/CPU), 페이지 범위,
  파이프라인 옵션 등 세부 제어가 가능합니다.
- 참고: 기본 사용은 내부 기본 컨버터를 사용하지만, 커스텀 파라미터가 필요하면 직접 만든 DocumentConverter를
  전달합니다 [[1]](https://github.com/docling-project/docling-langchain) [[3]](https://github.com/docling-project/docling).

#### 3) convert_kwargs (optional): any specific kwargs for conversion execution

- 값: dict 형태. converter.convert(...) 호출에 그대로 전달되는 키워드 인자.
- 역할: 변환 단계의 세부 옵션을 문서별로 제어합니다. 예: OCR 관련 설정, 처리 모드, 배치 크기, 페이지 범위 등(정확한 키는 Docling 버전에 따라 다를 수 있음).
- 주의: 사용 가능한 키는 사용하는 Docling 버전의 DocumentConverter/convert API에 따라 달라집니다. 확실치 않다면 converter=DocumentConverter(...)로 원하는
  옵션을 생성자에서 설정하는 방식을
  권장합니다 [[3]](https://github.com/docling-project/docling).

#### 4) export_type (optional): export mode to use: ExportType.DOC_CHUNKS (default) or ExportType.MARKDOWN

- 값: ExportType.DOC_CHUNKS 또는 ExportType.MARKDOWN.
- 역할:
    - DOC_CHUNKS(기본): 문서를 청킹하여 각 청크를 별도의 LangChain Document로 반환합니다. 이때 chunker가 사용됩니다.
    - MARKDOWN: 입력 문서당 하나의 LangChain Document를 반환하며, 내용은 Markdown 문자열입니다(후단에서 별도 분할이 필요할 수 있음).
- 참고: 두 모드 차이에 대한 설명은 공식 예시와 통합 가이드에 명시되어
  있습니다 [[2]](https://colab.research.google.com/github/langchain-ai/langchain/blob/master/docs/docs/integrations/document_loaders/docling.ipynb) [[3]](https://haystack.deepset.ai/integrations/docling).

#### 5) md_export_kwargs (optional): any specific Markdown export kwargs (for Markdown mode)

- 값: dict 형태. Markdown으로 내보낼 때 사용하는 옵션.
- 역할: ExportType.MARKDOWN 모드에서 DLDocument.export_to_markdown(...)에 전달됩니다. 이미지 플레이스홀더 등 Markdown 렌더링 방식을 조정할 수 있습니다.
- 예시: Docling 생태계의 다른 통합 예시(LlamaIndex)에서는 기본값으로 {"image_placeholder": ""}가 사용되는 모습을 볼 수 있습니다. 사용 가능한 키는 Docling의
  Markdown export 구현에 따릅니다 [[5]](https://docs.llamaindex.ai/en/stable/api_reference/readers/docling/).

#### 6) chunker (optional): any specific Docling chunker instance to use (for doc-chunk mode)

- 값: Docling의 청커 인스턴스(예: HybridChunker 등).
- 역할: ExportType.DOC_CHUNKS 모드에서 텍스트를 문서 구조/토큰 길이 기준 등으로 잘게 나누는 전략을 지정합니다.
- 팁: 토크나이저/최대 토큰 수 등을 설정해 임베딩 모델 입력에 맞추는 식으로 쓰는 예시가 문서에
  있습니다 [[2]](https://colab.research.google.com/github/langchain-ai/langchain/blob/master/docs/docs/integrations/document_loaders/docling.ipynb).

#### 7) meta_extractor (optional): any specific metadata extractor to use

- 값: Docling 메타데이터 추출기 인스턴스(타이틀, 저자, 섹션 헤딩, 페이지 정보 등).
- 역할: 변환/청킹된 결과에 풍부한 메타데이터를 추가하여 LangChain Document.metadata에 담습니다. 고급 RAG에서 유용합니다.
- 비고: 레포/가이드에서 선택적 인자로 언급됩니다. 구체 구현은 사용하는 extractor에 따라
  다릅니다 [[1]](https://github.com/docling-project/docling-langchain).

#### 간단 사용 예시

```python
# Python
from langchain_docling import DoclingLoader
from docling.document_converter import DocumentConverter
from docling_core.chunking import HybridChunker  # 실제 패키지 경로는 설치 버전에 따라 다를 수 있음

FILE_PATHS = [
    "https://arxiv.org/pdf/2408.09869",  # URL
    "/path/to/local/doc.pdf",  # 로컬 파일
]

# 1) 기본 설정: DOC_CHUNKS(기본) 모드, 내부 기본 컨버터/청커 사용
loader_basic = DoclingLoader(file_path=FILE_PATHS)
docs_basic = loader_basic.load()

# 2) 마크다운으로 단일 문서당 하나씩 받기 + Markdown export 옵션
loader_md = DoclingLoader(
    file_path=FILE_PATHS,
    export_type="MARKDOWN",  # 혹은 ExportType.MARKDOWN
    md_export_kwargs={
        "image_placeholder": "",  # 사용 가능한 키는 Docling 버전에 따름
    },
)
docs_md = loader_md.load()

# 3) 커스텀 컨버터 + 커스텀 청커 + 변환 kwargs
custom_converter = DocumentConverter(
    # 예: 디바이스/모드/ocr 등 세팅 (사용 가능한 옵션은 Docling 버전에 따름)
)
custom_chunker = HybridChunker(
    tokenizer="intfloat/e5-base",  # 예: 토크나이저/최대 길이 등 설정
    # max_tokens=..., overlap=..., 등
)

loader_advanced = DoclingLoader(
    file_path=FILE_PATHS,
    converter=custom_converter,
    convert_kwargs={
        # 예: 페이지 범위, OCR 언어, 처리 모드 등
        # "page_ranges": [(1, 5)],
        # "ocr_languages": ["eng"],
    },
    export_type="DOC_CHUNKS",  # 기본값
    chunker=custom_chunker,
    # meta_extractor=...,  # 필요 시 설정
)
docs_advanced = loader_advanced.load()
```

#### 추천 사용 가이드

- 빠르게 쓰고 싶다: file_path만 넘겨 기본값(DOC_CHUNKS)으로 시작하세요.
- 문서 전체를 Markdown으로 받고 싶다: export_type=MARKDOWN과 md_export_kwargs를 조정하세요.
- 고급 제어가 필요하다: converter(및 convert_kwargs), chunker를 직접 지정하세요.
- 메타데이터가 중요한 워크플로: meta_extractor를 통해 제목/섹션/페이지 정보 등 풍부한 메타를 Document.metadata에 담으세요.
