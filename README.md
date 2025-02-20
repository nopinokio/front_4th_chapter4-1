## 프론트엔드 배포 파이프라인

GitHub Actions에 워크플로우를 작성해 다음과 같이 배포가 진행되도록 합니다.

1. 저장소를 체크아웃합니다.
2. Node.js 18.x 버전을 설정합니다.
3. 프로젝트 의존성을 설치합니다.
4. Next.js 프로젝트를 빌드합니다.
5. AWS 자격 증명을 구성합니다.
6. 빌드된 파일을 S3 버킷에 동기화합니다.
7. CloudFront 캐시를 무효화합니다.

## 주요 링크

- s3 버킷 웹사이트 엔드포인트 
: http://amzn-s3-mijeon-bucket0.s3-website-ap-southeast-2.amazonaws.com/
- CloudFrount 배포 도메인 이름
: [d26vz7r83p6mdq.cloudfront.net](https://d26vz7r83p6mdq.cloudfront.net/)

## 주요 개념

- **GitHub Actions**과 CI/CD 도구: GitHub에서 제공하는 자동화된 워크플로우 도구입니다. 코드 변경사항이 발생할 때마다 자동으로 빌드, 테스트, 배포 등의 작업을 수행하여 지속적 통합(CI)과 지속적 배포(CD)를 가능하게 합니다.
- **S3와 스토리지**: Amazon S3(Simple Storage Service)는 AWS에서 제공하는 클라우드 스토리지 서비스입니다. 정적 웹사이트 호스팅이 가능하며, 높은 내구성과 가용성을 제공합니다. 이 프로젝트에서는 빌드된 Next.js 애플리케이션의 정적 파일들을 저장하고 서비스하는 데 사용됩니다.
- **CloudFront와 CDN**: Amazon CloudFront는 AWS의 콘텐츠 전송 네트워크(CDN) 서비스입니다. 전 세계에 분산된 엣지 로케이션을 통해 콘텐츠를 캐싱하고 더 빠른 속도로 사용자에게 전달합니다. S3에 저장된 정적 파일들을 전 세계 사용자들에게 빠르게 제공하는 역할을 합니다.
- **캐시 무효화(Cache Invalidation)**: CloudFront가 엣지 로케이션에 캐싱한 콘텐츠를 강제로 새로고침하는 프로세스입니다. 새로운 버전의 웹사이트가 배포될 때 실행되어, 사용자들이 항상 최신 버전의 콘텐츠를 받아볼 수 있도록 보장합니다.
- **Repository secret과 환경변수**: GitHub 저장소에서 안전하게 보관하는 민감한 정보들입니다. AWS 인증 정보와 같은 중요한 값들을 코드에 직접 노출시키지 않고 안전하게 사용할 수 있게 해줍니다. 이 프로젝트에서는 AWS 접근 키, 시크릿 키, 리전 정보 등을 저장하는데 사용됩니다.

## 개발 환경 설정

이 프로젝트는 `create-next-app`으로 생성된 Next.js 프로젝트입니다.

### 개발 서버 실행

```bash
npm run dev
# or
yarn dev
```