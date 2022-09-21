- 페이징
	- 데이터를 작게 나누어 로드하는데 적합한 라이브러리
	- 데이터를 필요한 만큼만 로드하면 자원과 네트워크 사용을 줄일 수 있다

- 페이징 컴포넌트 프로젝트에 추가
	- 의존성을 추가하기 위해 maven 저장소가 추가되어야 함
	- 그 뒤 모듈의 gradle에 의존성을 추가

- 다양한 데이터 구조 지원
	- 페이징 라이브러리는 다음과 같은 데이터 구조를 갖는 모델을 타깃으로 설계됨
		- 네트워크로부터 데이터 가져오기
		- 로컬 DB로 부터 데이터 가져오기
		- 네트워크에서 얻은 데이터를 로컬 DB에 캐싱하여 사용하기
	
- 구성요소
	- 페이징된 데이터를 가져오는 데 사용되는 구성요소로 DataSource와 PagedList가 있다.

- DataSource
	- 데이터를 작게 나누어 불러온 다음 PagedList로 로드하는 기본 클래스
	- 더많은 데이터를 로드할 수록 PagedList가 커지며 로드된 데이터는 갱신할 수 없음
	- 데이터의 변경이 발생하면 새로운 PagedList와 DataSource를 생성해야 함

- DataSourece.Factory
	- DataSource 인스턴스를 만들어 제공하는 팩토리 클래스

- PagedList
	- DataSource에서 페이징된 데이터들을 관리하는 리스트
	
- LivePagedListBuilder
	- LiveData<PagedList> 를 만드는 빌더 클래스

- PagedListAdapter
	- 페이징된 데이터를 표시하는 리사이클러뷰의 어댑터 기반의 클래스
	
- DataSource 종류 선택하기

- PositionalDataSource
	- 주소록 앱처럼 로컬에 데이터가 있지만 사용자가 임의의 위치로 이동하려는 경우에 적합
	- 실제로 Room이 내부적으로 사용하는 방식
	
- ItemKeyedDataSource
	- 유니크한 아이템 키를 페이징하는 DataSource의 서브클래스
	- 이전에 로드된 아이템의 키가 다음 아이템을 로드하는데 사용됨

- PageKeyedDataSource
	- 서버 사이드 API에서 가장 일반적인 페이징 방법
	- 클라이언트는 페이징 요청을 보내고 서버에서 페이징된 데이터를 포함한 응답을 내려줌 
