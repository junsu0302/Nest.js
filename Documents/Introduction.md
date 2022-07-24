# Nest.js

## Nest.js 특징

```md
1. node.js 서버 애플리케이션 구축 프레임워크
2. Express 기반 프레임워크
3. Augular의 아키텍처 사상을 기반으로 제작
4. 외부 모듈 사용 가능
```

## Nest.js 장점

```md
1. 아키텍처 구성의 소모되는 시간 단축
2. 다양한 라우팅, 보안 등 기능 사용 가능
3. 외부 모듈을 통한 확장 가능
```

## Nest.js 생성

```cmd
npm i -g @nestjs/cli
nest new project-name
```

생성된 프로젝트 구조의 소스 폴더는 다음과 같다.

```md
src
├─ app.controller.spec.ts
├─ app.controller.ts
├─ app.module.ts
├─ app.service.ts
└─ main.ts
```

```md
1. app.controller.spec.ts : controller 테스트
2. app.controller.ts : client의 request 처리, response 전송
3. app.module.ts : 모듈 정의(controller와 service 정의)
4. app.service.ts : controller가 요청한 비즈니스 로직을 처리
5. main.ts : 프로젝트 시작 소스
```

### main.ts
```ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```
```md
Application의 시작점

1. NestFactory를 사용하여 Application 인스턴스 생성
2. 생성 시 AppModule 사용
```

### app.module.ts
```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```
```md
main.ts에서 지정한 root module

1. import를 통해 모듈 가져옴
2. controllers : 이 모듈에서 사용하는 컨트롤러 세트
3. providers : 직접적으로 사용되는 class 지정
4. export를 통해 다른 모듈에서 사용 가능
```

### app.controller.ts
```ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```
```md
client의 요청에 대한 라우팅 처리

1. provider인 AppService에게 처리 요청 보냄
2. 받은 결과를 client에 응답으로 보냄
```

### app.service.ts
```ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```
```md
요청받은 비지니스 로직을 처리한 후 controller에게 반환
```

## 소스 자동 생성

### Module 자동 생성
```cmd
nest g module 모듈명
```

### Controller 자동 생성
```cmd
nest g controller 모듈명
```

### Service 자동 생성
```cmd
nest g service 모듈명
```

이 때 Service는 **@injectable()**데코레이터를 사용한다.

```md
1. DI(Dependency injection)개념 사용
2. Service를 사용할 곳에 선언해주면 필요한 시점에 자동으로 생성
```
