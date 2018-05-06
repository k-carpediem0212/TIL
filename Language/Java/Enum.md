* enum의 특이사항



enum은 생성자가 private 이므로 new로 새로운 인스턴스를 만들 수 없다.

newInstance() 에 의해 인스턴스를 만들 수 없다. newInstance()를 호출 하면 InstantiationException이 발생한다.

clone()을 만들 수 없다. Enum 클래스를 상속받고 있지만, clone() 메쏘드가 final로 지정되어 있어 사용하면 CloneNotSupportedException이 발생한다.

Serialize : enum의 값이 수정되어 순서가 변경되어도, ordinal 값이 다시 재 정의 된다.
