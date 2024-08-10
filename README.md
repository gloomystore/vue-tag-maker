# GloomyTags

`GloomyTags`는 태그를 추가하고 제거할 수 있는 Vue.js 컴포넌트입니다. 사용자가 입력 필드에 태그를 추가하거나 제거하는 기능을 제공합니다. 이 컴포넌트는 기본적인 태그 입력과 사용자 정의 스타일링, 입력 제어를 지원합니다.

## 설치

`GloomyTags` 컴포넌트를 Vue 프로젝트에 추가하려면 아래와 같이 설치할 수 있습니다.

```bash
npm install gloomy-tags
```

또는

```bash
yarn add gloomy-tags
```

## 사용법

1. **컴포넌트 등록**

   컴포넌트를 사용할 Vue 파일에서 다음과 같이 등록합니다.

   ```vue
   <script setup lang="ts">
   import GloomyTags from 'gloomy-tags'
   </script>

   <template>
     <GloomyTags
       :state="tags"
       :setState="updateTags"
       name="tags"
       placeHolder="Type and press enter"
     />
   </template>
   ```

2. **컴포넌트 속성**

   `GloomyTags` 컴포넌트는 다음과 같은 속성을 지원합니다:

   - **`name`**: 입력 필드의 이름입니다.
   - **`placeHolder`**: 입력 필드의 플레이스홀더 텍스트입니다.
   - **`state`**: 현재 태그 배열입니다.
   - **`setState`**: 태그 배열을 업데이트하는 함수입니다.
   - **`onChange`**: 태그가 변경될 때 호출되는 콜백 함수입니다.
   - **`onBlur`**: 입력 필드가 포커스를 잃을 때 호출되는 콜백 함수입니다.
   - **`separators`**: 태그를 구분하는 키 배열입니다. 기본값은 `[]`입니다.
   - **`disableBackspaceRemove`**: 백스페이스로 태그를 제거할 수 없도록 설정합니다. 기본값은 `false`입니다.
   - **`onExisting`**: 이미 존재하는 태그가 입력될 때 호출되는 콜백 함수입니다.
   - **`onRemoved`**: 태그가 제거될 때 호출되는 콜백 함수입니다.
   - **`disabled`**: 입력 필드를 비활성화합니다. 기본값은 `false`입니다.
   - **`isEditOnRemove`**: 태그 제거 시 편집 모드로 전환할지 여부입니다. 기본값은 `false`입니다.
   - **`beforeAddValidate`**: 태그를 추가하기 전에 호출되는 유효성 검사 함수입니다.
   - **`onKeyUp`**: 입력 필드에서 키가 눌릴 때 호출되는 콜백 함수입니다.
   - **`classNames`**: 커스터마이즈된 CSS 클래스 객체입니다.
   - **`throttleTime`**: 태그 추가 시 요청의 스로틀 시간(ms)입니다. 기본값은 `300`입니다.

3. **컴포넌트 메서드**

   - **`cursorToInput`**: `gloomy-tag--container` 클릭 시 입력 필드로 포커스를 이동시킵니다.
   - **`handleKeyDown`**: 키 입력 이벤트를 처리하여 태그를 추가하거나 제거합니다.
   - **`handleTagRemove`**: 특정 태그를 제거합니다.

4. **스타일링**

   `GloomyTags`는 scoped CSS를 사용하여 스타일을 적용합니다. 컴포넌트의 CSS는 다음과 같은 요소를 포함합니다:

   - `.gloomy-tag--container`: 태그 입력 컨테이너의 기본 스타일입니다.
   - `.gloomy-tag--input`: 입력 필드의 기본 스타일입니다.
   - `.gloomy-tag--tag`: 각 태그의 기본 스타일입니다.
   - `.gloomy-tag--focus`: 포커스된 태그의 스타일입니다.
   - `.gloomy-tag--tag.disappearing`: 제거될 태그의 스타일입니다.

## 예제

```vue
<template>
  <div id="app">
    <GloomyTags
      :state="tags"
      :setState="updateTags"
      name="tags"
      placeHolder="Type and press enter"
      :separators="['Enter', ',']"
      :classNames="{ tag: 'custom-tag-class', input: 'custom-input-class' }"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import GloomyTags from 'gloomy-tags'

const tags = ref<string[]>([])

function updateTags(newTags: string[]) {
  tags.value = newTags
}
</script>
```

## Contributing

기여를 원하시는 분은 [CONTRIBUTING.md](CONTRIBUTING.md)를 참조해주세요.

## License

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조해주세요.
