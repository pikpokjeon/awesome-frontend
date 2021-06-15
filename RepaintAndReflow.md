# Reflow Repaint

## reflow

예를들어, div>span 여러 다양한 형태의 요소들이 있다.
여기서
상위 div는 span요소에 따라 최종적으로 판단됨
상위는 자식엘리먼트의 영향을 받음
div 형제들은 자기 형 노드의 영향을 받음
어떻게 repaint를 적게할것인가? 변경사항을 한쪽으로 몰아준다. 
변경이 자주일어날 곳은 브라우저에서 디바운싱한다. 유발인자들만 한 함수에 작성해준다.

아래와 같은경우 좋지 못한 예임으로 속성 변화는 한곳이 몰아주는것이 맞다
```
div.style.width = '96px';
let c = a + b;
div.style.height = '48px'
```
position이 absolute인경우는 자식에만 영향.
전체 리플로우를 막울수 있다.

```
div.getBoundingClientRect()
div.offsetWidth
div.getComputedStyle();

```
호출되는 수만큼 리플로우가 일어난다. 필요한 순간 몰아서 계산해야한다.
정확한 정보를 찾기위함으로, 다시 그리는 것이아니라 repaint가 일어나지 않는다.

-> reflow는 복싱에서 쨉과 같다 


## repaint

엘리먼트가 변해서 새로 그려줘야한다 값이 더 비싸다.(실행비용이 비쌈) 
예를들면
캔버스위에 svg, html 에니메이션!! 또는 fixed 한 position의 요소가 opacity를 가져서 겹친 앨리먼트를 또한 그려줘야 하는경우..
회피방법이 거의 없다. 그래서 리플로우 관련 성능 최적화 방법이 많다.


-> repaint는 복싱으로 따지면 훅날리는거..