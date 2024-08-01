npm i -g @nestjs/cli
nest new my-project
cd my-project
npm install --save @nestjs/typeorm typeorm mysql2

1. npm i -g @nestjs/cli
2. nest new ub_Server
오류 발생시 
관리자 권한으로 PowerShell을 실행할 수 없거나 현재 사용자에 대한 실행 정책만 변경하려는 경우 CurrentUser 범위를 사용할 수 있습니다. 이 범위 변경에는 관리 권한이 필요하지 않으며 현재 사용자에게만 영향을 미칩니다.
PowerShell 창에서 다음 명령을 실행합니다.
powershell
-Set-ExecutionPolicy RemoteSigned -Scope CurrentUser

3.npm install --save @nestjs/typeorm typeorm mysql2
4.npm install --save class-validator class-transformer

5. uuid 모듈 사용하기  
   유니크 값 사용시 사용 대부분 ID 아이디에 사용
 - npm install uuid --sav

6. 파이프 필요한 모듈
   파이프를 이용해서 게시물을 생성할 때 유효성 체크
- npm install class-validator class-transformer --save
참조
- https://github.com/typestack/class-validator#manual-validation

8. TypeORM을 사용하기 위해서 설치해야하는 모듈
@nestjs/typeorm
 -Nest.js에서 TypeOrm을 사용하기 위해 연동
npm install pg typeorm @nestjs/typeorm --save
//passport-jwt는 JWT를 검증하고 해독
npm install @nestjs/passport @nestjs/jwt passport-jwt


리포지토리는 데이터 계층에 대한 추상화를 제공하므로 SQL 쿼리를 직접 처리하지 않고도 객체 지향 방식으로 데이터베이스와 상호 작용할 수 있습니다. 이 추상화는 다음과 같습니다.

데이터 액세스 논리를 단순화합니다.
모든 데이터 관련 작업(CRUD)을 한 곳에 캡슐화합니다.
비즈니스 로직을 변경하지 않고도 데이터 소스를 쉽게 교체할 수 있습니다.

예에서 'BoardsService'는 'boardsRepository'와 상호작용하여 다양한 데이터 작업을 수행합니다. 이 구조는 데이터 관련 로직이 저장소 계층에 깔끔하게 캡슐화되므로 애플리케이션을 확장 가능하고 유지 관리하기 쉽게 만듭니다.

 TypeORM 버전 0.3에서 @EntityRepository 데코레이터가 지원되지 않기 때문에, DataSource와 Repository를 사용하여 커스텀 리포지토리를 구현하는 방법을 보여줍니다. boardRepository는 AppDataSource에서 생성된 BoardRepository 인스턴스를 사용합니다. 이 방법은 최신 TypeORM 버전에서 권장되는 방식입니다.

/////////////////////////////////////////////////////////////////////////////////////////////////////

 DataSource는 TypeORM 0.3 이상 버전에서 데이터베이스 연결을 관리하고 데이터베이스와의 상호작용을 처리하는 핵심 객체입니다. DataSource를 생성하고 사용하는 이유는 다음과 같습니다:

1. 데이터베이스 연결 관리
DataSource는 데이터베이스와의 연결을 설정하고 관리합니다. 여기에는 데이터베이스 유형, 호스트, 포트, 사용자명, 비밀번호, 데이터베이스 이름 등의 설정이 포함됩니다. 이러한 설정은 TypeOrmConfig 객체에서 정의되며, DataSource를 통해 데이터베이스 연결이 초기화됩니다.

2. 엔티티 및 스키마 동기화
DataSource는 프로젝트의 모든 엔티티를 관리하고, 데이터베이스 스키마와 애플리케이션의 엔티티 정의를 동기화합니다. synchronize 옵션을 true로 설정하면 애플리케이션 시작 시 데이터베이스 스키마가 엔티티 정의와 자동으로 동기화됩니다. 이는 개발 중 데이터베이스 구조를 쉽게 관리할 수 있게 해줍니다.

3. 리포지토리 관리
TypeORM에서 리포지토리는 데이터베이스에 대한 CRUD(생성, 읽기, 업데이트, 삭제) 작업을 수행하는 데 사용됩니다. DataSource는 각 엔티티에 대해 기본 리포지토리를 제공하며, 이를 통해 데이터베이스와의 상호작용을 처리합니다. 또한, 커스텀 리포지토리를 정의할 때도 DataSource를 통해 리포지토리를 확장할 수 있습니다.

4. 마이그레이션 및 로깅
DataSource는 마이그레이션을 처리하는 데도 사용됩니다. 마이그레이션은 데이터베이스 스키마의 변경 사항을 관리하고 추적하는 기능입니다. DataSource 객체에서 마이그레이션을 설정하여 데이터베이스 스키마를 관리할 수 있습니다. 또한, logging 옵션을 통해 쿼리 로깅 및 데이터베이스 작업 로깅을 활성화할 수 있습니다.

/////////////////////////////////////////////////////////////////////////////////////////////////////

리포지토리 패턴 

1. 데이터베이스 설정 파일 생성
   src/configs/TypeORM.config.ts 

2. 엔티티 정의
   MySQL에서 사용할 엔티티를 정의합니다. 여기서는 User 엔티티를 정의합니다.
   src/users/user.entity.ts

3. 리포지토리 설정
   TypeORM을 사용하여 리포지토리를 설정합니다. 
   src/users/users.repository.ts
   
4. 서비스 계층
   비즈니스 로직을 처리하는 서비스 계층입니다.
   src/users/users.service.ts

5. 모듈 설정
   모듈 설정으로 TypeORM 모듈과 컨트롤러, 서비스, 리포지토리를 결합합니다.
   src/users/users.module.ts
   
6. UsersController
   컨트롤러를 통해 요청을 처리하고 응답합니다.
   src/users/users.controller.ts


이해
constructor(private readonly dataSource: DataSource) {
    this.boardRepository = this.dataSource.getRepository(Board);
  }

constructor(private readonly dataSource: DataSource):
constructor는 클래스의 인스턴스가 생성될 때 호출되는 함수입니다.
여기서는 DataSource 객체를 private readonly로 주입받고 있습니다. private는 이 속성이 클래스 내부에서만 접근 가능하도록 하고, readonly는 이 속성이 할당된 후 변경할 수 없음을 나타냅니다.
DataSource는 TypeORM에서 데이터베이스 연결을 관리하는 객체로, 다양한 데이터베이스 작업을 수행할 수 있는 기능을 제공합니다.
this.boardRepository = this.dataSource.getRepository(Board);:

dataSource 객체의 getRepository 메서드를 호출하여 Board 엔티티에 대한 Repository 객체를 가져옵니다.
Repository<Board> 객체는 Board 엔티티와 관련된 데이터베이스 작업(CRUD 작업 등)을 수행하는 데 사용됩니다.
이 Repository 객체는 TypeORM이 제공하는 기능을 통해 데이터베이스에서 Board 엔티티의 데이터를 저장, 조회, 업데이트, 삭제하는 데 사용됩니다.
역할 및 기능
DataSource:
TypeORM의 중심 객체로, 데이터베이스 연결을 관리하고, 리포지토리 및 엔티티와의 상호작용을 처리합니다.
DataSource는 애플리케이션의 전체 수명 주기 동안 유지되는 단일 연결 풀을 관리합니다.
getRepository(Entity):
특정 엔티티(Board와 같은)에 대한 Repository 객체를 반환합니다.
이 Repository 객체는 해당 엔티티의 인스턴스를 데이터베이스에 저장하거나 데이터베이스로부터 검색하는 등의 작업을 수행하는 데 사용됩니다.


커스텀 파이프 구현 방법

먼저 PipeTransform이란 인터페이스를 새롭게 만들 커스텀 파이프에 구현해줘야 합니다. 이 PipeTransform 인터페이스는 모든 파이프에서 구현해줘야 하는 인터페이스입니다. 그리고 이것과 함께 모든 파이프는 transform() 메소드를 필요합니다. 이 메소드는 NestJS가 인자(arguments)를 처리하기 위해서 사용됩니다.

import { ArgumentMetadata, PipeTransform } from '@nestjs/common';

export class BoardStatusValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    console.log('value', value)
    console.log('metadata', metadata)

    return value;
  }
}