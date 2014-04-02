node-hwp
========
본 제품은 한글과컴퓨터의 한/글 문서 파일(.hwp) 공개 문서를 참고하여 개발하였습니다.

HWP 버전 5 문서를 여는 Node.js 라이브러리를 만드는 시도를 하는 중입니다.

현재, 이 패키지는 대부분의 한글 문서를 읽을 수 있게 읽을 수 있는 객체 종류를 계속 늘리고 있으며, 아직 다른 프로젝트에 사용될 준비가 되어 있지 않습니다.

라이센스는 `LICENSE.md`를 참고 해 주세요.

작업 중 목록
------------
여기 있는 것은 당연히 모든 것을 포함하는 목록이 아닙니다.

* Node.js 외의 환경에서도 (특히 웹 브라우저) 사용 가능하게 하기.
* 파일
	* 암호화, 배포용 문서 읽는 방법 알아내고 읽기.
* Head
	* TAB_DEF 더 자세히 채워넣기.
	* BULLET 완성하기.
	* 기타 다른 빠진 곳 채워넣기.
* Body
	* PARA_LINE_SEG의 "더 이상의 자세한 설명은 생략한다" 부분 채워넣기.
	* 영역 태그 처리하고, 종류를 찾아 분류해 보기.
	* 컨트롤 오브젝트 더 읽기.
	* 기타 다른 빠진 곳 채워넣기.
* 기타 `test/read.js`의 주석에 적혀 있는 의문점들 해결하기.
* Import / Export
	* HWP로 읽어온 다음 HWPML로 내보낼 때 원본이 최대한 유지되게 하기.
	* HWPML에서 읽어오는 기능 추가하기.
	* HWP로 내보내는 기능 추가하기.

API
---
우선 간단하게 다음과 같이 HWP 파일을 읽어와 HWPML로 출력할 수 있습니다.
```js
var hwp = require('hwp');
hwp.open('file.hwp', function(err, doc){
	console.log(doc.toHML());
});
```

### `hwp.open(file, [type], callback)`
주어진 경로로부터 HWP 문서를 읽습니다.

__인자__

* file : HWP 문서의 경로입니다.
* type (선택) : HWP 문서가 저장된 형식입니다. 현재는 `hwp`만을 지원합니다.
* callback(err, doc) : 콜백 함수입니다. 함수가 호출될 때에 첫 번째 인자에는 파일을 읽어올 때 발생한 에러 (있다면), 두 번째 인자에는 HWP 파일 객체가 대입됩니다.

---------------------------------------------------

### `hwp.HWP`
HWP 문서를 나타내는 객체입니다. 아래와 같은 속성들과 메소드들이 있습니다.

#### `_doc`
HWP 문서로부터 문서를 읽어왔다면, 이 속성에 원본 문서를 나타내는 오브젝트가 저장됩니다.

#### `_hml`
HWPML같은 형식으로 (루트는 `<HWPML>`) HWP 문서가 표현되어 있습니다. (아래 `HWPNode`를 참조 해 주세요.)

#### `_hwp_meta`
HWP 문서로부터 문서를 읽어왔다면, 이 속성에 그 문서의 메타데이터가 저장됩니다.

#### `toHML([verbose])`
HWP 문서를 HWPML형식으로 표현한 문자열을 반환합니다.

__인자__

* verbose (선택) : true라면 보기 좋게 공백과 개행문자를 넣어줍니다.

---------------------------------------------------

### `HWPNode`
직접적으로 드러나 있지는 않지만, `hwp.HWP` 객체의 `_hml`을 표현하는 데 사용되는 프로토타입입니다.

#### `name`
노드의 이름입니다.

#### `attr`
노드의 속성을 담고 있는 오브젝트입니다.

#### `value`
노드의 텍스트 값입니다.

#### `children`
노드의 자식들을 포함하고 있는 배열입니다.

#### `add(elem)`
노드에 `elem`을 자식으로 추가하고, 만약 `attr.Count` 속성이 있을 경우 그것을 업데이트 합니다.

#### `findChild(name)`, `go(name)`
노드에서 주어진 이름을 가지고 있는 첫 번째 자식 노드를 찾아 반환합니다. 없을 경우 `null`을 반환합니다.

#### `getChild(name)`
`findChild(name)`과 같으나, 만약 그러한 노드가 없을 경우 주어진 이름을 가지는 새 노드를 추가하고 그것을 반환합니다.

#### `findChildWith(name, attr_name, attr_val)`
노드에서 주어진 이름을 가지고 있고, 주어진 인자의 값이 주어진 값인 노드를 찾아 반환합니다. 없을 경우 `null`을 반환합니다.

#### `getChildWith(name, attr_name, attr_val)`
`findChildWith(name, attr_name, attr_val)`과 같으나, 만약 그러한 노드가 없을 경우 해당하는 새 노드를 추가하고 그것을 반환합니다.

#### `findChildren(name)`
노드에서 주어진 이름을 가지고 있는 모든 자식 노드들을 반환해 줍니다.

#### `toHML([verbose])`
노드를 HWPML형식으로 표현한 문자열을 반환합니다.
