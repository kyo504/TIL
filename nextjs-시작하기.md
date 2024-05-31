## 로컬에 Next.JS 템플릿 프로젝트를 생성

```
npx create-next-app@latest zmsm-web
```

그러면 아래와 같이 옵션이 나오는데 일단 기본으로 모두 설정

```
✔ Would you like to use TypeScript? … No / Yes
✔ Would you like to use ESLint? … No / Yes
✔ Would you like to use Tailwind CSS? … No / Yes
✔ Would you like to use `src/` directory? … No / Yes
✔ Would you like to use App Router? (recommended) … No / Yes
✔ Would you like to customize the default import alias (@/*)? … No / Yes
```

## Github 에 프로젝트를 생성하고 remote 로 push

Github 에 빈 repository 를 생성한 다음에 로컬에서 remote 를 설정하고 현재 상태를 push 한다.

```
git remote add origin https://github.com/kyo504/zmsm-web.git
```

## Vercel 에서 import 를 통해서 프로젝트 생성

Vercel 에서 import 를 통해서 위에서 만든 프로젝트를 연결한다.

여기까지 하면 zmsm-web 에서 main 에 push 한 내용은 vercel 에서 자동으로 빌드 및 배포를 한다.

## 도메인 연결

도메인은 godaddy 에서 구매했고 이를 vercel 과 연결하여 구매한 도메인으로 접근시 redirect 되도록 한다. 해당 내용은 아래 링크를 통해서 진행한다.
https://www.codecrafthub.co.uk/blog/goDaddyVercel

