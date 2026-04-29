
## 운영방식

```
1) 전역 규칙
AGENTS.md

2) Copilot 기본 지침
.github/copilot-instructions.md

3) 영역별 지침
.github/instructions/ui.instructions.md
.github/instructions/testing.instructions.md
.github/instructions/monorepo.instructions.md

4) 재사용 프롬프트
.github/prompts/*.prompt.md

5) 사람용 설명서/체크리스트
docs/ai/*.md
```

### Add Context (컨테스트 추가 - # 활용)

채팅 변수를 사용하여 프롬프트에 특정 컨텍스트를 포함할 수 있다. 예를 들어, `AGENTS.md` 파일에 프로젝트의 주요 기술 스택이나 아키텍처 패턴을 명시해두면, 챗봇이 그 내용을 참고해서 대화할 때 더 정확하고 일관된 답변을 제공할 수 있다.

### 슬래시 명령어 

코파일럿 챗에서 슬래시 명령어를 활용하여 특정 작업을 요청할 수 있다. 예를 들어, `/test` 명령어를 입력하면 테스트 케이스 작성에 대한 프롬프트가 자동으로 불러와져서 일관된 방식으로 테스트 케이스를 작성할 수 있다.

> 슬래시 명령을 사용하면 일반적인 시나리오에 대한 복잡한 프롬프트를 작성하지 않도록 한다. 슬래시 명령을 사용하려면 채팅 프롬프트 상자에 `/`를 입력한 다음, 명령 이름을 입력합니다.

### 채팅 참가자

> 채팅 참가자는 도움이 될 수 있는 전문 분야를 갖춘 도메인 전문가와 같다. 채팅 프롬프트 상자에 `@`을 입력하고 그 뒤에 채팅 참가자 이름을 입력하여 채팅 참가자를 지정할 수 있습니다. 사용 가능한 모든 채팅 참가자를 보려면 채팅 프롬프트 상자에 `@`을(를) 입력합니다.

---
## 각 파일의 역할과 활용 방법

### 1. Copilot Instructions (코파일럿 기본 지침)

코파일럿이 코드를 생성하거나 자동 완성할 때 **항상 기본적으로 따라야 하는 전반적인 규칙**을 설정한다.즉 전역적으로 코드를 생성할 때의 기본 스타일, 아키텍처 패턴, 기술 스택, 코드 품질 기준 등을 명시하는 것이다. 이 지침은 프로젝트의 루트 디렉토리에 `.copilot-instructions.md` 파일로 만들어두면 된다.

- **활용 예시:** * "항상 TypeScript를 사용하고 타입 정의를 엄격하게 해줘."
    - "변수명은 항상 camelCase로 작성해 줘."
    - "React 컴포넌트는 함수형으로 만들고 화살표 함수를 사용해 줘."

### 2. Instruction Files (파일별/작업별 지침 파일)

> https://awesome-copilot.github.com/learning-hub/defining-custom-instructions/

특정 파일 확장자나 특정 작업 유형에 맞춰 세부적인 규칙을 적어둔 `.instructions.md` 파일을 설정한다. 프로젝트 내에 이 파일을 만들어두면, 코파일럿이 해당 파일을 편집할 때 그 안의 규칙을 참고한다.

> 코드베이스의 특정 파일이나 디렉터리를 사용할 때 GitHub Copilot의 동작을 자동으로 안내하는 ==영구 구성 파일==이다. 사용자가 직접 호출하거나 에이전트가 실행해야 하는 스킬과 달리, 사용자 지정 지침은 백그라운드에서 작동하여 Copilot이 팀의 표준, 규칙 및 아키텍처 결정을 일관되게 준수하도록한다.

> 지침은 하위 항목에 `instructions/`있으며 전역, 언어별 또는 디렉터리별로 glob 패턴을 사용하여 범위를 지정할 수 있다. 이러한 지침은 Copilot이 엔지니어링 플레이북에 자동으로 맞춰 작동하도록 도와준다.

>  특정 파일이나 디렉터리에서 작업할 때 자동으로 적용되는 지속적인 컨텍스트를 제공
>  **사용 시점** : 프로젝트 전체의 코딩 표준, 아키텍처 패턴 또는 기술별 규칙 등 모든 제안에 영향을 미쳐야 하는 사항에 사용합니다

- 코딩 표준 또는 스타일 가이드(명명 규칙, 테스트 전략)
- 프레임워크별 힌트(예: React, Node.js, Monorepo 등)
- 저장소별 규칙("비밀 정보는 절대 커밋하지 마세요", "기능 플래그는 에 있어야 합니다 `flags/`").
-  구조 설계 결정 : 프로젝트 구조, 디자인 패턴, 관례
- 준수 요구사항 : 보안 정책, 규제 제약

#### 잘 구성된 지침 파일에는 다음 내용이 포함된다.

1. **명확한 제목 및 개요** : 이 지침서에서 다루는 내용
2. **구체적인 지침** : 모호한 제안이 아닌 실행 가능한 규칙
3. **코드 예시** : 올바른 패턴을 보여주는 작동 코드 조각
4. **설명** : 특정 접근 방식이 선호되는 이유

``` md
---
description: 'Playwright test automation with TypeScript'
applyTo: '**/*.spec.ts, **/tests/**/*.ts'
---

# Playwright Testing Standards

Write descriptive test names that explain the expected behavior.

## Test Structure

```typescript
test('should display error message when login fails', async ({ page }) => {
  await page.goto('/login');
  await page.fill('#username', 'invalid');
  await page.fill('#password', 'invalid');
  await page.click('#submit');

  await expect(page.locator('.error')).toBeVisible();
});
```

#### 모범 사례

- **파일당 하나의 목적** : 보안, 테스트, 스타일링 등 서로 다른 고려 사항에 대한 별도의 지침을 작성한다.
- **명확한 이름 지정 방식을 사용** : 파일 이름은 설명적으로 지정한다. 예를 들어 `react-component-standards.instructions.md`, `name`이 아닌 `name`을 사용하세요.`rules.instructions.md`
- **예제 포함** : 모든 가이드라인에는 최소 하나 이상의 코드 예제가 포함되어야 한다.
- **최신 상태를 유지하세요** : 종속성이나 프레임워크가 업데이트될 때마다 지침을 검토하세요.
- **명령어를 테스트하세요** : 코드를 생성하고 Copilot이 패턴을 따르는지 확인.
- **관련 문서 링크** : 자세한 설명은 공식 문서를 참조.
- **규칙에는 표를 사용하세요** : 표 형식은 명명 규칙 및 비교에 효과적이다.
#### 지침 vs 스킬

- 지침은 수동적인 맥락을 제공하고, 기술은 함께 제공되는 리소스를 활용하여 특정 작업을 수행하도록 한다.
- 반복적으로 적용되는 표준에 대해서는 지침을 사용하고, 필요에 따라 작동하는 작업에는 기술을 활용한다.

### 3. AGENTS.md 

> https://awesome-copilot.github.com/learning-hub/building-custom-agents/

사용자 지정 에어전트는 GitHub Copilot에 특화된 페르소나, 특정 도구 접근 권한 및 도메인 전문 지식을 제공하는 전문 도우미이다. 수동적으로 적용되는 지침이나 개별 작업을 처리하는 스킬과는 달리, 에이전트는 완전한 작업 스타일을 정의한다. 즉, Copilot이 생각하는 방식, 사용하는 도구, 그리고 전체 세션 동안 소통하는 방식을 결정한다.

채팅 창(Copilot Chat)에서 대화할 때, 프로젝트의 전반적인 아키텍처나 주요 기술 스택, 도메인 지식 등을 코파일럿에게 주입하기 위해 사용하는 파일이다. 루트 디렉토리에 `AGENTS.md`를 만들어두면 챗봇이 그 내용을 배경지식으로 깔고 대화한다.

- 저장소에서 작얼할 때 항상 지켜야 하는 기본 원칙이다.
- 아키텍처, 테스트, 출력 스타일, 검증 순서 등을 명시할 수 있다.


> **대화(새로운 섹션)시 사용자 지정 에이전트를 선택**하거나 Copilot 코딩 에이전트를 통해 이슈에 에이전트를 할당하면 에이전트 구성이 전체 상호 작용에 영향을 미친다. 즉 에이전트를  선택하면, 해당 에이전트의 페르소나, 도구 액세스 및 가드레일이 적용되어 일관된 행동과 출력을 제공한다. 코파일럿 챗과의 대화시 섹션을 만들고 섹션에게 해당 에이전트를 할당하여, 챗봇과의 대화에서 해당 에이전트의 행동과 출력을 활용할 수 있다.

 사용자 지정 에이전트를 선택하거나 Copilot 코딩 에이전트를 통해 이슈에 에이전트를 할당하면 에이전트 구성이 전체 상호 작용에 영향을 미친다.
 
 - 즉 에이전트를  선택하면, 해당 에이전트의 페르소나, 도구 액세스 및 가드레일이 적용되어 일관된 행동과 출력을 제공한다.
 - 코파일럿 챗과의 대화시 섹션을 만들고 섹션에게 해당 에이전트를 할당하여, 챗봇과의 대화에서 해당 에이전트의 행동과 출력을 활용할 수 있다.
#### 커스텀 에이전트란?

`*.agent.md`사용자 지정 에이전트는 GitHub Copilot을 다음과 같이 구성하는 Markdown 파일입이다.

- 페르소나: 에이전트가 채택하는 전문성, 어조 및 작업 스타일
- **Tool access**: 에이전트가 사용할 수 있는 내장 도구 및 MCP 서버
- **가드레일**: 에이전트가 따르는 경계 및 관례
- **모델 선호도**: 에이전트에 사용할 AI 모델 (선택 사항이지만 권장됨)
#### 핵심 사항

- 에이전트는 대화 전반에 걸쳐 지속성을 유지하며, 세션이 진행되는 동안 일관된 행동과 출력을 제공한다.
- 에이전트는 도구를 호출하고, 명령을 실행하고, 코드베이스를 검색하고, MCP 서버와 상호작용 할 수 있다.
- 하나의 저장소에는 여러 에이전트가 공존할 수 있으며, 각 에이전트는 서로 다른 워크플로 또는 도메인 전문 지식을 지원하도록 구성할 수 있다.
- `github/agents/`에이전트는 전체 팀에 저장 되고 공유된다.

#### 에이전트 지침

``` markdown
---
name: 'Security Reviewer'
description: 'Expert security auditor that reviews code for OWASP vulnerabilities, authentication flaws, and supply chain risks'
model: Claude Sonnet 4
tools: ['codebase', 'terminal', 'github']
---
```

- **이름** (권장): 에이전트에 표시할 사람이 읽기 쉬운 이름입니다.
- **설명** (필수): 상담원이 하는 일을 명확하게 요약한 내용입니다. 이 내용은 상담원 선택기에 표시되어 사용자가 적합한 상담원을 찾는 데 도움이 됩니다.
- **모델** (권장): 에이전트를 구동하는 AI 모델입니다. 작업의 복잡성에 따라 선택하세요. 미묘한 추론에는 더 강력한 모델을 사용하는 것이 좋습니다.
- **도구** (권장): 에이전트가 액세스할 수 있는 내장 도구 및 MCP 서버 모음입니다. 일반적인 도구는 다음과 같습니다.

|도구|목적|
|---|---|
|`codebase`|저장소 전체의 코드를 검색하고 분석합니다.|
|`terminal`|셸 명령 실행|
|`github`|GitHub API(이슈, PR 등)와 상호 작용합니다.|
|`fetch`|외부 API에 HTTP 요청을 보냅니다.|
|`edit`|워크스페이스의 파일을 수정하세요|

``` md
---
name: 'API Design Reviewer'
description: 'Reviews API designs for consistency, RESTful patterns, and team conventions'
model: Claude Sonnet 4
tools: ['codebase', 'github']
---

# API Design Reviewer

You are an expert API designer who reviews endpoints, schemas, and contracts for consistency and best practices.

## Your Expertise

- RESTful API design patterns
- OpenAPI/Swagger specification
- Versioning strategies
- Error response standards
- Pagination and filtering patterns

## Review Checklist

When reviewing API changes:

1. **Naming**: Verify endpoints use plural nouns, consistent casing
2. **HTTP Methods**: Confirm correct verb usage (GET for reads, POST for creates)
3. **Status Codes**: Check appropriate codes (201 for creation, 404 for not found)
4. **Error Responses**: Ensure structured error objects with codes and messages
5. **Pagination**: Verify cursor-based pagination for list endpoints
6. **Versioning**: Confirm API version is specified in the path or header

## Output Format

Present findings as:
- 🔴 **Breaking**: Changes that break existing clients
- 🟡 **Warning**: Patterns that should be improved
- 🟢 **Good**: Patterns that follow our conventions

```

#### 모범 사례

- **전문성을 구체적으로 명시** . "React 18+ 및 TypeScript 전문가"라고 쓰는 것이 "프론트엔드 개발자"라고 쓰는 것보다 좋다.
- **업무 스타일을 정의** . 상담원은 명확한 질문을 해야 할까요, 아니면 추측을 해야 할까요? 간결해야 할까요, 아니면 꼼꼼해야 할까요?
- **안전장치를 포함** : 에이전트가 절대 해서는 안 되는 일은 무엇인가요? ("프로덕션 구성 파일을 직접 수정해서는 안 됩니다.")
- **예시를 제공** : 기대하는 출력 형식을 보여주세요 (리뷰 주석, 코드 패턴 등).

에이전트들이 한 가지 일에 집중하도록 한다. 파일당 하나의 페르소나를 부여하는 것이 좋다. 에이전트 하나가 너무 많은 작업을 처리하려고 하면, 여러 에이전트로 분할하거나 공통 작업을 에이전트가 사용할 수 있는 스킬로 분리한다.

#### 에이전트 vs 지침

- 에이전트는 명시적으로 선택되며, 지침은 일치하는 파일에 자동으로 적용된다.
- 에이전트는 완전한 페르소나를 정의하고, 지침은 수동적인 배경 맥락을 제공한다.
- 대화형 워크플로에는 에이전트를 사용하고, 코딩 표준 및 규칙에는 지침을 사용한다.
#### 에이전트 vs 스킬

- 에이전트는 지속적인 페르소나이며, 스킬은 단일 작업 수행 능력이다.
- 챗봇은 대화 중에 스킬을 사용할 수 있다.
- 복잡하고 여러 단계를 거치는 워크플로우에는 에이전트를 사용하고, 집중적이고 반복적인 작업에는 스킬을 사용한다.

### 4.Prompts (프롬프트)

코파일럿에게 특정 작업을 요청할 때, 재사용할 작업 템플릿이다.
주로 버그 수정, 리뷰 코멘트 작성, 코드 리팩토링 등 특정 작업을 요청할 때 활용한다. 프롬프트는 `.copilot-prompts.md` 파일로 만들어두고, 챗봇과 대화할 때마다 그 템플릿을 참고해서 일관된 요청을 할 수 있다.

```
/ 명령어로 요청 할 수 있다.

ex) /test
```

### 5.Skills (스킬)

> https://awesome-copilot.github.com/learning-hub/creating-effective-skills/

스킬은 재사용 가능한 기능을 담은 독립적인 폴더이다.

스킬은 재사용 가능한 기능(지침, 참조 파일, 템플릿, 스크립트 등)을 하나의 단위로 묶은 독립형 폴더이다. 에이전트는 이를 자동으로 검색하고 사용자는 슬래시 명령어를 통해 호출할 수 있다. 스킬을 통해 팀은 테스트 생성, 코드 검토, 문서 작성과 같은 일반적인 워크플로를 표준화하여 모든 팀 구성원이 일관되고 높은 품질의 결과를 얻을 수 있도록 지원합니다.

> 스킬은 폴더에 참조 파일, 템플릿 및 스크립트를 묶어서 저장할 수 있으므로 단일 파일보다 AI에 더 풍부한 컨텍스트를 제공한다. 기존 프롬프트 형식과 달리 에이전트는 스킬을 자동으로 검색하고 호출할 수 있다.
> 
> **사용 시점** : 테스트 생성, 문서 작성, 패턴 리팩토링 등 팀에서 **정기적으로 수행하는 반복적인 작업**에 사용.
> **스킬은 사용자 또는 에이전트에 의한 명시적인 호출이 필요하다.**

> AI에 더 풍부한 컨텍스트를 제공하기 위해, 스킬은 특정 작업을 수행하는 데 필요한 도구와 지침을 하나의 페르소나로 묶어 제공한다. 예를 들어, "React 전문가" 스킬을 만들어서 React 관련 질문에 대해 깊이 있는 답변을 제공하도록 할 수 있다. 이는 도구와 지침을 하나의 페르소나로 묶어 제공한다.

#### 스킬이란?

스킬은 파일과 선택적으로 번들로 제공되는 에셋을 포함하는 폴더이다.

- **이름** : 사용자가 호출할 때 사용되는 케밥 케이스 형식의 식별자 `/command`(예: `/generate-tests`)
	- 슬래시 명령어로 호출할 때 사용된다.
- **설명** : 해당 스킬의 기능과 발동 조건
	- 해당 스킬을 언제 사용해야 하는지 명확히 설명한다.
- **지침** : Copilot이 실행하는 상세 워크플로
- **자산 참조** : 번들로 제공되는 템플릿, 스크립트, 스키마 및 참조 문서에 대한 링크

``` md
---
name: generate-tests
description: 'Generate comprehensive unit tests for the selected code, covering happy path, edge cases, and error conditions'
---

# generate-tests

Generate comprehensive unit tests for the selected code.

## When to Use This Skill

Use this skill when you need to create or expand test coverage for existing code.

## Requirements

- Cover happy path, edge cases, and error conditions
- Use the testing framework already present in the codebase
- Follow existing test file naming conventions
- Include descriptive test names explaining what is being tested
- Add assertions for all expected behaviors
  
## Testing Patterns

Follow the patterns documented in [references/testing-patterns.md](references/testing-patterns.md).

Use [templates/test-template.ts](templates/test-template.ts) as a starting structure.
  
```

**1. 명확한 목표

```
# skill-name
Your goal is to [specific task] for [specific target].
```

**2. "언제 사용해야 하는지"에 대한 안내를 추가한다** (에이전트 및 사용자 모두에게)

```
## When to Use This Skill
Use this skill when:- A user asks to [trigger phrase 1]- You need to [trigger phrase 2]- Keywords: [keyword1], [keyword2], [keyword3]
```

**3. 요구사항을 명확하게 정의**

```
## Requirements
- Must follow [standard/pattern]- Should include [specific element]- Avoid [anti-pattern]
```

**4. 참조 번들 자산**

```
## References
- Follow patterns in [references/patterns.md](references/patterns.md)- Use template from [templates/starter.json](templates/starter.json)
```

**5. 예를 들어 설명하세요** 

```
### Good Example[Show desired output]
### What to Avoid[Show problematic patterns]
```

#### 모범 사례

- **스킬당 하나의 목적** : 하나의 작업 또는 워크플로에 집중
- **검색을 유도하는 글을 작성하세요** : 핵심 키워드를 활용하여 설명글을 작성하면 에이전트가 적합한 스킬을 쉽게 찾을 수 있다.
- **중요한 것들을 묶으세요** : 착각을 줄여주는 템플릿, 스키마, 참조 문서를 포함
- **범용성을 확보하세요** : 다양한 프로젝트에서 활용할 수 있는 스킬을 작성
- **명확하게 표현하십시오** : 모호한 표현을 피하고 정확한 요구 사항을 명시
- **설명적인 이름을** 사용하세요: 명확하고 행동 지향적인 이름을 사용하세요 `generate-tests`.`helper`
- **파일 크기를 최소화하세요** : 번들로 제공되는 파일은 각각 5MB 미만이어야 합니다.
- **철저한 테스트** : 다양한 입력값과 코드베이스를 사용하여 기능이 제대로 작동하는지 확인하십시오.

#### 번들 자산 추가

스킬에는 AI의 컨텍스트를 풍부하게 하는 참조 파일, 템플릿 및 스크립트가 포함될 수 있다.

```
skills/
└── generate-tests/
    ├── SKILL.md
    ├── references/
    │   └── testing-patterns.md      # Common testing patterns
    ├── templates/
    │   └── test-template.ts         # Starter test file
    └── scripts/
        └── setup-test-env.sh        # Environment setup
```


#### 기존 프롬프트 파일 형식 대비 주요 장점

- 스킬 지원 기능은 에이전트 검색을 위한 확장된 메타데이터(이름, 설명)를 포함한다.
- 에이전트는 스킬을 자동으로 찾아 호출할 수 있으며, 이전에는 수동으로 슬래시 명령어를 입력해야 했다.
- 스킬은 지침과 함께 추가파일(참조 문서, 템플릿, 스크립트 등)을 포함할 수 있어, 단일 프롬프트 파일보다 AI에 더 풍부한 컨텍스트를 제공한다.
- 개발형 에이전트 스킬 사양을 통해 코딩 에이전트 시스템 전반에 걸쳐 스킬이 더욱 표준화되었다.
- 스킬은 프롬프트와 마찬가지로 슬래시 명령어로 호출할 수 있지만, 에이전트가 자동으로 검색하여 호출할 수도 있다.

#### 스킬 vs 지침

- 스킬은 에이전트 또는 사용자에 의해 명시적으로 호출되며, 지침은 자동으로 적용된다.
- 스킬은 패키지화된 리소스를 활용하여 특정 작업을 수행하는 데 필수적이며, 지침은 지속적인 맥락을 제공한다.
- 필요에 따라 실행되는 워크플로에는 스킬을 사용하고, 항상 적용되는 표준에는 지침을 사용한다.

#### 스킬 vs 에이전트

- 스킬은 특정 작업에 특화된 역량이고, 에이전트는 특정 도메인이나 기술 스택에 특화된 행동을 하도록 설정할 수 있다(가장 전문적이면 주관적이다.).
- 스킬은 표준 Copilot 도구와 연동되며 자체 자산을 번들로 제공한다. 에이전트는 MCP 서버 또는 사용자 지정 통합이 필요할 수 있다.


---
## 상호작용

1.지침은 지속되는 안정장치를 마련한다. 코파일럿이 코드를 생성할 때마다, 지침을 참고하여 일관된 스타일과 품질을 유지한다.

2.스킬 및 프롬프트는 필요에 따라 풍부하고 재사용 가능한 워크플로를 실행할 수 있으며, 특정 작업을 요청할 때마다 일관된 프롬프트를 활용하여 효율성을 높인다.

3.에이전트는 특정 도메인이나 기술 스택에 특화된 행동을 하도록 설정할 수 있다(가장 전문적이면 주관적이다.). 예를 들어, "React 전문가" 에이전트를 만들어서 React 관련 질문에 대해 깊이 있는 답변을 제공하도록 할 수 있다. 이는  도구와 지침을 하나의 페르소나로 묶어 제공한다.
