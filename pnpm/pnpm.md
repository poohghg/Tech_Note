#### 사용이유

https://pnpm.io/ko/motivation

하나의 패키지는 TypeScript나 lodash 등 여러 패키지를 가져와 만들게 됩니다(dependency). 하나의 패키지를 설치한다고 해도 실제로는 꼬리에 꼬리를 무는 dependency 패키지까지 모두 설치하는 셈입니다.

이를 모두 node_modules에 저장하면 복잡하고 무겁기에, npm과 Yarn은 복잡하게 얽힌 dependency들을 단일 루트 하에 평평하게 위치시킵니다(Hoisting). 하지만 이때문에 중복된 패키지가 저장되거나, 내가 설치한 적 없는 패키지를 쓸 수 있게 되어 잠재적인 버그 혹은 보안 사고의 여지가 있습니다.

pnpm은 이 문제를 해결하기 위해 node_modules를 직접 설치하는 대신, 전역 저장소(Virtual Store)에서 패키지를 공유하는 구조를 사용합니다. pnpm이 패키지를 설치할 때, package.json에 명시된 패키지를 읽은 후 node_modules에 ==Symbolic Link(symlinks)를 생성하여 전역 저장소의 해당 패키지를 참조합니다==. 이 방식을 통해 명시한 패키지만 사용할 수 있게 됩니다.

부가적으로 얻는 장점도 컸습니다. 동시에 중복된 패키지를 설치하지 않아 저장 공간과 네트워크를 절약할 수 있었으며, Hard link와 Symbolic Link(symlinks)를 이용하여 파일 복사를 최소화하여 더 빠르게 패키지를 설치할 수 있게 되었습니다.

pnpm의 경우에는 프로젝트별로 node_modules에 매번 패키지를 설치했던 것과는 달리 **global 저장소에 패키지를 한 번만 저장함으로써 저장 공간을 절약할 수 있다는 아주 큰 장점**을 가지고 있다.
즉, pnpm을 사용한다면 똑같은 라이브러리를 중복해서 설치할 필요가 없다는 의미다.