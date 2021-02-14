[참고 블로그](https://nshipster.co.kr/swift-gyb/)
[GYP](https://gyp.gsrc.io/)

> 파이썬 코드를 이용하여 변수를 대체하거나 흐름을 제어한다.

```jsx
%{ code } 
- 파이썬 코드 블록

% code: ... % end
- 흐름 제어

${ code }
- 표현식 결과 대체
```

```swift
// Codable.swift.gyb
%{
codable_types = ['Bool', 'String', 'Double', 'Float',
                 'Int', 'Int8', 'Int16', 'Int32', 'Int64',
                 'UInt', 'UInt8', 'UInt16', 'UInt32', 'UInt64']
}%

// 이터레이터 생성
% for type in codable_types:
  mutating func encode(_ value: ${type}) throws
% end

// 결과
mutating func encode(_ value: Bool) throws
mutating func encode(_ value: String) throws
mutating func encode(_ value: Double) throws
mutating func encode(_ value: Float) throws
mutating func encode(_ value: Int) throws
mutating func encode(_ value: Int8) throws
mutating func encode(_ value: Int16) throws
mutating func encode(_ value: Int32) throws
mutating func encode(_ value: Int64) throws
mutating func encode(_ value: UInt) throws
mutating func encode(_ value: UInt8) throws
mutating func encode(_ value: UInt16) throws
mutating func encode(_ value: UInt32) throws
mutating func encode(_ value: UInt64) throws
```

# Xcode에서는 어떻게?

- 내장이 아니라서 `xcrun`에선 사용 불가.
- `chmod 커맨드`를 이용해서 사용가능

    ```bash
    wget https://github.com/apple/swift/raw/master/utils/gyb
    wget https://github.com/apple/swift/raw/master/utils/gyb.py
    chmod +x gyb
    ```

    - `wget`: HTTP/S, FTP 프로토콜을 통한 웹 페이지 파일 다운로드
    - `chmod`: **ch**ange **mod**e로, 디렉토리나 파일의 모드(권한)를 변경
- Xcode 내에서 프로젝트 활성 타겟 선택 → `Build Phase` → `Add New Run Script Phase`

    ```bash
    find . -name '*.gyb' |                                               \
        while read file; do                                              \
            ./path/to/gyb --line-directive '' -o "${file%.gyb}" "$file"; \
        done
    ```