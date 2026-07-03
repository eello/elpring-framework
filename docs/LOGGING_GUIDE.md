# Elpring Framework 로깅 가이드라인 (Logging Guide)

이 문서는 `elpring-di`, `elpring-web`, `elpring-boot` 프로젝트에 일관된 로깅을 적용하기 위한 규칙을 정의합니다.

## 로깅 의존성 및 기본 원칙
1. 프레임워크 라이브러리는 `org.slf4j:slf4j-api`만을 사용하여 로깅을 수행합니다.
   - `elpring-di`의 `build.gradle`에 `api` 형태로 추가하여 상위 모듈이 자연스럽게 로깅을 사용할 수 있도록 구성합니다.
2. 성능 저하를 막기 위해 로깅 시 문자열 결합(String concatenation, `+` 연산)은 사용하지 않으며 치환자(`{}`)를 사용합니다.
3. 무거운 객체 생성이 필요한 로그는 `if (log.isDebugEnabled())` 처럼 레벨 체크 블록으로 감싸 성능을 최적화합니다.
4. `System.out.println` 및 `e.printStackTrace()`는 프레임워크 런타임에서 절대 사용하지 않습니다.

## 로그 레벨 (Log Level) 가이드
- **INFO**: 
  - 프레임워크 핵심 라이프사이클 마일스톤 및 상태 요약
  - 예: 컨테이너 초기화 시간, 구동 서버 포트 정보, 등록된 빈 개수, 초기 매핑된 URL 목록 등
- **DEBUG**:
  - 정상적인 동작 과정 추적 및 트러블슈팅
  - 예: 스캔된 후보 컴포넌트 정보, 빈 생성 시작/종료 과정, ArgumentResolver 파라미터 매칭, DispatcherServlet 요청 매핑 결과
- **TRACE**:
  - 상세 탐색 로직 및 딥다이빙 (복잡한 매칭 과정 추적)
  - 예: 의존성 파싱 시 `@Primary` 판별 흐름, 제네릭 컬렉션 의존성 매칭 내부 로직
- **WARN / ERROR**:
  - 예: 빈 생성 실패(`BeanCreationException`), 순환 참조 감지, 포트 바인딩 실패, 잘못된 `@RequestMapping` 중복 설정

## 로거 선언 규약
각 클래스의 상단에 다음과 같이 로거를 선언합니다.
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class SomeClass {
    private static final Logger log = LoggerFactory.getLogger(SomeClass.class);
}
```
